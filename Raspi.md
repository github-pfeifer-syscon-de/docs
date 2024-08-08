
# Raspi

Nice to see what is possible with Raspi4 cardsize computer, enough power to email, browse and play music. But if you think about some serious work (development, using IDE), it definitly needs some cooling concept.

## Error detection

There is a nice page for debugging:

[pi not booting](https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=58151)

And for a pi4 the power supply is crucial, it might look fine if the led is on, but better check with a multimeter +5/3.3V (like in the old days). And 4.5V on the 5V line is definetly insufficent.

## Console
```

```
If you get a broken console display there is the help:

[Config.txt](https://www.raspberrypi.org/documentation/configuration/config-txt/)

Most important:

```
config_hdmi_boost=7
```

## Logic

Need some hardware debugging help see:

[Logic analysator style program](http://wp11237257.server-he.de/wiki/index.php/Gtk3#Logic)

## Play webvideo

[chroium drm](https://ubuntu-mate.community/t/tutorial-chromium-netflix-and-other-drm-video-websites/7185)

But this is rather old, tried to get some newer version by extracting the conf ... [extract script](https://gist.github.com/ruario/19a28d98d29d34ec9b184c42e5f8bf29).

[list recovery files](https://dl.google.com/dl/edgedl/chromeos/recovery/recovery.conf)

## Arch Arm

Alternative to Raspian.

[Arch Arm](https://archlinuxarm.org/platforms/armv8/broadcom/raspberry-pi-4)

with a overall nice compatibility. This leaves the choice of packages (and the need to search) to you.

### Device permission

But has some rather relaxed security standards, you may want to add: 

```
# limit block device permission (like x64 arch)
SUBSYSTEM=="block", KERNEL=="mmcblk0", ACTION=="add", RUN+="/bin/chmod o-rwx /dev/$name"
SUBSYSTEM=="block", KERNEL=="mmcblk0p1", ACTION=="add", RUN+="/bin/chmod o-rwx /dev/$name"
SUBSYSTEM=="block", KERNEL=="mmcblk0p2", ACTION=="add", RUN+="/bin/chmod o-rwx /dev/$name"
SUBSYSTEM=="block", KERNEL=="mmcblk0p3", ACTION=="add", RUN+="/bin/chmod o-rwx /dev/$name"
SUBSYSTEM=="block", KERNEL=="mmcblk0p4", ACTION=="add", RUN+="/bin/chmod o-rwx /dev/$name"
```

to /etc/udev/rules.d/05-block.rules to limit the read permission for partition devices (if you want to take it event further add ",g-w").

## Java

The Open-Jdk/Jre is based on the ZeroVM (that misses some special platform optimization). 
So if you seriously want to use Java give the original Oracle package a try (its like giving your raspi a pair of rocket boots ;).
All other platforms shoud still be fine with the open stuff.

Meanwhile there is be a open source alternative: https://adoptium.net/

## Linux console

### fix messages spilling on console

Add to /boot/cmdline.txt:

```
quiet loglevel=3
```

Add file 20-quiet-printk.conf in /etc/sysctl.d

```
kernel.printk = 3 3 3 3
```

### Prevent overheating

(Newer kernels don't drive so obviously into thermal limit)

Use some small heat sink, they are just preventing the worst.

Add to /boot/cmdline.txt:

```
isolcpus=2,3
```

Keeps it below 80°C some effect but quite harsh.

Add to /boot/config.txt:

```
arm_freq=800
over_voltage=-6
```

Result ~75°, feels like talking to a shuffling zombie.

```
arm_freq=1000
over_voltage=-4
```

Cant get this into a stable state. 

Without a cooling solution this makes a nice system for mailing and surfing, but your old Pc will not be soon retiered if you use some application that will really need to compute something.

And the setup for remote developing with netbeans you may use (Samba, ssh, X-fowarding) helps but need to find an option to debug X-Applications.

But as always there might be some smarter solution like geany nice source editor, but gdb has a steep learning curve...

## Compile kernel modul

```
[http://www.tldp.org/LDP/lkmpg/2.6/html/hello2.html Kernel compile infos]
```

```
#  pacman -S linux-raspberrypi4-headers
```

### Compile kernel modul

And for what this is good? Here is a blinking Kernel Modul (it offers you the option per defines to use older/newer gpio functions):

hello.h

```
/*
*  Prototypes
*/
int init_module(void);
void cleanup_module(void);
static int device_open(struct inode *, struct file *);
static int device_release(struct inode *, struct file *);
static ssize_t device_read(struct file *, char *, size_t, loff_t *);
static ssize_t device_write(struct file *, const char *, size_t, loff_t *);
#define SUCCESS 0
#define DEVICE_NAME "chardev"    /* Dev name as it appears in /proc/devices   */
#define CLASS_NAME "chardev"
#define BUF_LEN 80        /* Max length of the message from the device */
```

hello.c

```
/*
*  hello_gpio demonstrate gpio access in kernel-module.
*  
* From:
* http://www.tldp.org/LDP/lkmpg/2.6/html/x1052.html
* Gpio from:
* http://derekmolloy.ie/kernel-gpio-programming-buttons-and-leds/
*/
#include <linux/kernel.h>
#include <linux/module.h>
#include <linux/fs.h>
#include <asm/uaccess.h>    /* for put_user */
#include <linux/uaccess.h>    /* copy_from_user */
#include <linux/hrtimer.h>
#include <linux/sched.h>
#include <linux/gpio.h>                 // Required for the GPIO functions
#include <linux/gpio/consumer.h>        // Required for the new gpiod functions
#include "hello.h"
#include "bcm2835_gpio.h"
// the defines allow to switch functionallities
// #define KERN_GPIO
//    #define KERN_GPIOD -> newer gpiod functions
//    #undef  KERN_GPIOD -> older gpio functions
// #undef KERN_GPIO -> bare metal taste, port bcm2835 lib required
#undef KERN_GPIO    // define to use kernel gpio functions
#define KERN_GPIOD    // use gpiod if def, gpio otherwise
/*
* Global variables are declared as static, so are global within the file.
*/
static int Major;        /* Major number assigned to our device driver */
static int Device_Open = 0;    /* Is device open?
                * Used to prevent multiple access to device */
static char msg[BUF_LEN];    /* The msg the device will give when asked */
static char *msg_Ptr;
static char *msg_out_ptr;
static char *morse_ptr;
static enum hrtimer_restart function_timer(struct hrtimer *);
static struct hrtimer htimer;
static ktime_t kt_periode;
static char morse[512] = {    // ITU
   'a','.','-','\0','\0','\0','\0',
   'b','-','.','.','.','\0','\0',
   'c','-','.','-','.','\0','\0',
   'd','-','.','.','\0','\0','\0',
   'e','.','\0','\0','\0','\0','\0',
   'f','.','.','-','.','\0','\0',
   'g','-','-','.','\0','\0','\0',
   'h','.','.','.','.','\0','\0',
   'i','.','.','\0','\0','\0','\0',
   'k','-','.','-','\0','\0','\0',
   'l','.','-','.','.','\0','\0',
   'l','-','-','\0','\0','\0','\0',
   'n','-','.','\0','\0','\0','\0',
   'o','-','-','-','\0','\0','\0',
   'p','.','-','-','.','\0','\0',
   'q','-','-','.','-','\0','\0',
   'r','.','-','.','\0','\0','\0',
   's','.','.','.','\0','\0','\0',
   't','-','\0','\0','\0','\0','\0',
   'u','.','.','-','\0','\0','\0',
   'w','.','-','-','\0','\0','\0',
   'x','-','.','.','-','\0','\0',
   'y','-','.','-','-','\0','\0',
   'z','-','-','.','.','\0','\0',
   '1','.','-','-','-','-','\0',
   '2','.','.','-','-','-','\0',
   '3','.','.','.','-','-','\0',
   '4','.','.','.','.','-','\0',
   '5','.','.','.','.','.','\0',
   '6','-','.','.','.','.','\0',
   '7','-','-','.','.','.','\0',
   '8','-','-','-','.','.','\0',
   '9','-','-','-','-','.','\0',
   '0','-','-','-','-','-','\0',
   '\0','\0','\0','\0','\0','\0','\0'
   };
// Blinks on RPi Plug P1 pin 14 (which is GPIO pin 8 (for raspi 4))
static unsigned char gpioLED = 14;       ///< hard coding the LED gpio for this example to Pin 8 (GPIO14)
static unsigned int loop;
static struct class*  ebbcharClass  = NULL; ///< The device-driver class struct pointer
static struct device* ebbcharDevice = NULL; ///< The device-driver device struct pointer
#ifdef KERN_GPIOD
struct gpio_desc *gpioDesc;
#endif
static struct file_operations fops = {
   .read = device_read,
   .write = device_write,
   .open = device_open,
   .release = device_release
};
static char * find_morse(char chr) {
   char *tbl;
   //printk(KERN_INFO "searching morse code %c", chr);
   if (chr >= 'A' && chr <= 'Z') {
       chr += 0x20;    // convert to lowercase
   }
   for (tbl = morse; *tbl != '\0'; tbl += 7) {
       if (*tbl == chr) {
           //printk(KERN_INFO "got code %s", tbl);
           return tbl+1;
       }
   }
   printk(KERN_INFO "no code");
   return NULL;
}
static enum hrtimer_restart function_timer(struct hrtimer * unused)
{
   int value,n;
    /* do your timer stuff here */
   value = 0;
   if (msg_out_ptr != NULL
    && morse_ptr != NULL) {
       if (*morse_ptr == '\0') {    // end of morse char entry reached
           if (*msg_out_ptr == '\0') {
               msg_out_ptr = NULL;
               morse_ptr = NULL;
           }
           else {
               morse_ptr = find_morse(*msg_out_ptr++);
           }
       }
       if (morse_ptr != NULL) {
           ++loop;
           if (loop < 2) {
               // inbetween
           }
           else {
               value = 1;
               n = *morse_ptr == '-' ? 4 : 3;
               if (loop >= n) {
                   ++morse_ptr;
                   loop = 0;
               }
           }
       }
   }
   //printk(KERN_INFO "Loop %d.\n", loop);            // use to debug
   #ifdef KERN_GPIO
   // Turn it on/off
   #ifdef KERN_GPIOD
   gpiod_set_value(gpioDesc, value);
   #else
   gpio_set_value(gpioLED, value);
   #endif
   #else
   bcm2835_gpio_write(gpioLED, value ? HIGH : LOW);
   #endif
   hrtimer_forward_now(&htimer, kt_periode);
   return HRTIMER_RESTART;
}
/*
* This function is called when the module is loaded
*/
int init_module(void)
{
   htimer.function = NULL;
   msg_out_ptr = NULL;
   morse_ptr = NULL;
   loop = 0;
   #ifdef KERN_GPIO
       #ifdef KERN_GPIOD
       // use the number to identify pin seems the easiest solution...
       gpioDesc = gpio_to_desc(GPIOnum);
       if (!gpioDesc) {
           printk(KERN_INFO "GPIO_TEST: invalid GPIO line\n");
           return -ENODEV;
       }
       int rv;
       rv = gpiod_direction_output(gpioDesc, GPIOD_OUT_HIGH);
       if (rv) {
           printk(KERN_INFO "GPIO_TEST: invalid gpiod_direction_output\n");
           return -ENODEV;
       }
       #else
       // Is the GPIO a valid GPIO number (e.g., the BBB has 4x32 but not all available)
       if (!gpio_is_valid(gpioLED)){
         printk(KERN_INFO "GPIO_TEST: invalid LED GPIO\n");
         return -ENODEV;
       }
       // Going to set up the LED. It is a GPIO in output mode and will be on by default
       gpio_request(gpioLED, "sysfs");          // gpioLED is hardcoded to 14, request it
       gpio_direction_output(gpioLED, true);   // Set the gpio to be in output mode and on
       // gpio_set_value(gpioLED, ledOn);          // Not required as set by line above (here for reference)
       gpio_export(gpioLED, false);             // Causes gpioNN to appear in /sys/class/gpio
       // the bool argument prevents the direction from being changed
   #endif
   #else
   if (!bcm2835_init()) {
       printk(KERN_INFO "GPIO_TEST: cound not init bcm2835\n");
      return -ENODEV;
  }
       // Set the pin to be an output
   bcm2835_gpio_fsel(gpioLED, BCM2835_GPIO_FSEL_OUTP);
   #endif
    Major = register_chrdev(0, DEVICE_NAME, &fops);
   if (Major < 0) {
     printk(KERN_ALERT "Registering char device failed with %d\n", Major);
     return Major;
   }
   printk(KERN_INFO "I was assigned major %d. \n", Major);
   // Register the device class
   ebbcharClass = class_create(THIS_MODULE, CLASS_NAME);
   if (IS_ERR(ebbcharClass)){                // Check for error and clean up if there is
       unregister_chrdev(Major, DEVICE_NAME);
       printk(KERN_ALERT "Failed to register device class\n");
       return PTR_ERR(ebbcharClass);          // Correct way to return an error on a pointer
   }
  // Register the device driver
  ebbcharDevice = device_create(ebbcharClass, NULL, MKDEV(Major, 0), NULL, DEVICE_NAME);
  if (IS_ERR(ebbcharDevice)){               // Clean up if there is an error
     class_destroy(ebbcharClass);
     unregister_chrdev(Major, DEVICE_NAME);
     printk(KERN_ALERT "Failed to create the device\n");
     return PTR_ERR(ebbcharDevice);
  }
  printk(KERN_INFO "device /dev/%s registered.\n", DEVICE_NAME);
   //                          m  u  n
   kt_periode = ktime_set(0, 100000000ll); //seconds,nanoseconds
   hrtimer_init (& htimer, CLOCK_REALTIME, HRTIMER_MODE_REL);
   //if (htimer) {
       htimer.function = function_timer;
       hrtimer_start(& htimer, kt_periode, HRTIMER_MODE_REL);
   //}
   //else {
   //    printk(KERN_INFO "Error on timer init.\n");
   //}
   return SUCCESS;
}
/*
* This function is called when the module is unloaded
*/
void cleanup_module(void)
{
   msg_out_ptr = NULL;
   /* remove kernel timer when unloading module */
   if (htimer.function)
       hrtimer_cancel(& htimer);
   //if (ret < 0)
   //    printk(KERN_ALERT "Error in unregister_chrdev: %d\n", ret);
   #ifdef KERN_GPIO
   #ifdef KERN_GPIOD
       int rv;
       rv = gpiod_direction_input(gpioDesc);
       if (rv) {
         printk(KERN_INFO "GPIO_TEST: invalid line_input\n");
       }
       gpiod_put(gpioDesc);
   #else
       gpio_set_value(gpioLED, 0);              // Turn the LED off, makes it clear the device was unloaded
       gpio_unexport(gpioLED);                  // Unexport the LED GPIO
       gpio_free(gpioLED);                      // Free the LED GPIO
   #endif
   #else
       printk(KERN_INFO "bcm2835_close: revert to input\n");
       bcm2835_gpio_fsel(gpioLED, BCM2835_GPIO_FSEL_INPT);
       printk(KERN_INFO "bcm2835_close: close\n");
       bcm2835_close();
   #endif
   printk(KERN_INFO "bcm2835_close: device_destory\n");
   device_destroy(ebbcharClass, MKDEV(Major, 0));     // remove the device
   printk(KERN_INFO "bcm2835_close: class_unreg\n");
   class_unregister(ebbcharClass);                          // unregister the device class
   //printk(KERN_INFO "bcm2835_close: class_destory\n");
   //class_destroy(ebbcharClass);                             // remove the device class, but brings up refptr error, so we live better without it
   printk(KERN_INFO "bcm2835_close: unreg_char\n");
   unregister_chrdev(Major, DEVICE_NAME);
   printk(KERN_INFO "bcm2835_close: finished\n");
}
/*
* Methods
*/
/*
* Called when a process tries to open the device file, like
* "cat /dev/mycharfile"
*/
static int device_open(struct inode *inode, struct file *file)
{
   static int counter = 0;
   if (Device_Open)
       return -EBUSY;
   Device_Open++;
   sprintf(msg, "I already told you %d times Hello world!\n", counter++);
   msg_Ptr = msg;
   //my_device_data.size = sizeof(my_device_data.buffer);
   //file->private_data = &my_device_data;
   try_module_get(THIS_MODULE);
   return SUCCESS;
}
/*
* Called when a process closes the device file.
*/
static int device_release(struct inode *inode, struct file *file)
{
   Device_Open--;        /* We're now ready for our next caller */
   /*
    * Decrement the usage count, or else once you opened the file, you'll
    * never get get rid of the module.
    */
   module_put(THIS_MODULE);
   return 0;
}
/*
* Called when a process, which already opened the dev file, attempts to
* read from it.
*/
static ssize_t device_read(struct file *filp,    /* see include/linux/fs.h   */
              char *buffer,    /* buffer to fill with data */
              size_t length,    /* length of the buffer     */
              loff_t * offset)
{
   /*
    * Number of bytes actually written to the buffer
    */
   int bytes_read = 0;
   /*
    * If we're at the end of the message,
    * return 0 signifying end of file
    */
   if (*msg_Ptr == 0)
       return 0;
   /*
    * Actually put the data into the buffer
    */
   while (length && *msg_Ptr) {
       /*
        * The buffer is in the user data segment, not the kernel
        * segment so "*" assignment won't work.  We have to use
        * put_user which copies data from the kernel data segment to
        * the user data segment.
        */
       put_user(*(msg_Ptr++), buffer++);
       length--;
       bytes_read++;
   }
   /*
    * Most read functions return the number of bytes put into the buffer
    */
   return bytes_read;
}
/*
* Called when a process writes to dev file: echo "hi" > /dev/hello
*/
static ssize_t
device_write(struct file *file, const char *user_buffer, size_t size, loff_t * offset)
{
   ssize_t len = min(BUF_LEN - *offset, size);
   //printk(KERN_INFO "device_write off %llx len %x size %x.\n", *offset, len, size);
   if (len <= 0) {
       return 0;
   }
   /* read data from user buffer to my_data->buffer */
   if (copy_from_user(msg + *offset, user_buffer, len)) {
       printk(KERN_ALERT "Error copy from user.\n");
       return -EFAULT;
   }
   //msg[*offset + len + 1] = '\0';
   msg_out_ptr = msg;
   morse_ptr = find_morse(*msg_out_ptr++);
   *offset += len;
   return len;
}
MODULE_LICENSE("GPL");
MODULE_AUTHOR("Roland Pfeifer");
MODULE_DESCRIPTION("Do some blinking");
```

And a acompaining Makefile.

```
TARGET = blink
OBJS = hello.o
obj-m += $(TARGET).o
$(TARGET)-objs += $(OBJS)
default:
   make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules
clean:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean
```

With "echo "sos" > /dev/chardev" and a led connected to the declard pin you will get the morse representation of the text.
Dont get me wrong, its far easier to get this done from userspace, but if you are interested in doing some advanced functions. This might get you into the right direction....

Complete source with bare metal bcm2835-lib (c) Mike McCauley License: Open Source Licensing GPL V2

[Media:module.zip|Module source](Media:module.zip|Module source.md)

And why blinking? Well this was one of the first things i did on my first single board computer a UMS-85 the program had a handfull of hexcodes (8085 machine instructions) and after reading the mc-Magazin it took me 15 Minutes. The above is a result of searching dozends of web-pages with lose ends, discontinued packages. Brave new times.

## Direct Gpio access

[Gcc asm](https://gcc.gnu.org/onlinedocs/gcc/Extended-Asm.html)

[Gcc arm parameter passing](http://www.ethernut.de/en/documents/arm-inline-asm.html)

[Arm memory barriers](http://infocenter.arm.com/help/topic/com.arm.doc.dai0321a/DAI0321A_programming_guide_memory_barriers_for_m_profile.pdf)

Fastest output (Raspi4 11Mhz ;) that's a bit faster than 8 bit (Requires previous init of bcm2835-lib):

```
bcm2835_gpio_fsel(18, BCM2835_GPIO_FSEL_OUTP);
asm (
 "	ldr r0, =bcm2835_gpio	        // ptr addr\n"
 "	ldr r1, [r0]			// load ptr\n"
 "	mov r2, #1<<18			// set PIN 18\n"
 "	add r3, r1, #0x1C		// GPSET0\n"
 "	add r4, r1, #0x28		// GPCLR0\n"
 "set_pin:\n"
 "	str r2,[r3]			// set PIN 18 @ GPSET0\n"
 "clear_pin:\n"
 "	str r2,[r4]			// set PIN 18 @ GPCLR0\n"
 "	b   set_pin			// branch to beginning of loop\n"
 : 
 : [bcm2835_gpio]"r"   (bcm2835_gpio) 	          /* value get mapped gpio address */
 : "r0", "r1", "r2", "r3", "r4", "cc", "memory"  /* reg, condition reg, memory clobbers */
 );
```

## Using Gpio edge-sense

To prevent programms that use the gpio intensivly / use edge sense fen/ren from crashing (whole Raspi) use the following config:

```
/boot/config.txt
#dtparam=audio=on
dtoverlay=gpio-no-irq
```

## Packer

Dont care for everyday use, but if you spend long times compressing something this might point you in the right direction.

### Binary data (elf executable)

The number in braces is the command line option -n.


| Name |Min (1)   |Med (6)    |Max (9)     |preference
| --- | --- | --- | --- | --- |
|gzip  |41.7% 2.0s|38.1% 4.9s |37.9% 8.9s  |compatibility|
|bzip2 |37.0% 7.2s|<mark>35.3% 10.9s</mark>|35.1% 10.5s |overall|
|xz    |<mark>28.8% 8.3s</mark>|28.2% 12.4s(2)|         |size|
|zstd  |42.0% 0.5s|35.7% 3.6s|<mark>34.6% 5.2s   </mark>|speed but still tuneable |
|lz4  |56.2% 0.2s|           |45.5% 6.9s(12)|speed|
|gpg  |43.3% 1.6s|40.6% 2.9s |40.6% 4.0s    |speed encrypting|

So what is the best packer? You have to find it yourself as the performance differs by application and how you weight time vs. space and the used enviroment cores,... (if you pack purely random data most packers will not even pack). These tests are made on a reasonable sized executable. 
Some packers (lz4, gpg) are quite fast but do not quench out the last byte. 
Others (xz) reach a fantastic compression ratio, but is easy to be pushed too far (and then will consume a large amount of time and not getting considerably better results).
From my point of view bzip2 has a good overall performance (not too slow, and not such a bad ratio, not 
much of an option to get misconfigured, or if you put bzip2 in standard mode as a benchmark only two others (xz, zstd) with tuned options will do a better job).

### Source archive

Larger source-code archive (129MByte uncompressed, user-time):


| Name  |Pack ||Unpack
| --- | --- | --- | --- |
|bzip2  |11.9% 93.3s|18.6s |
|xz -1  | 9.5% 22.6s|3.5s|
|zstd -9| 8.7% 11.7s|0.8s|

Not much of an issue, but shows the advantages of xz and zstd more clearly.

Next test compression with artifical testdata:
* limited symbols (e.g. base64)
* repeating blocks (varing length, number of presets)

### Architecture

The characteristics for each packer are also obvious if varying the option over a greater range and testing on arm and x86.

Xz reaches the best size already with the 0 or 1 option (the other options i woud say are only useful if you pack it once and unpack it a few hundert times).

Bzip2 does its job almost without being biased by the option, (the default is just fine).

Zstd is the most flexible packer, it can be nicely tuned by the -n option.

The needed time grows non linear (rater exponential even if there seem to be some limits for xz and bzip2), where the gain on size is linear in the best case.

![pack time arm](/images/packTimeArm.png)

The x86 (Pentium(c)) architecture has a real advantage on packing, an older Athlon(c) is closer to Arm. I think this is because packing is done mostly from caches.

![pack time x86](/images/packTimeX86.png)

The gain on size is nearly linear (just to put it positivly).

![pack ratio arm](/images/packRatioArm.png)

As the packed binary was used of the architecture it was tested for, and the packing ratio on x86 is a little less effective. The source of the x86 plattform is 11% larger than that of the arm due to the 64/32bit difference. The effect of the entropy of the cisc (x86) vs that of the risc (arm) seems neglectable.

![pack ratio x86](/images/packRatioX86.png)


## Vulkan

```
pacman -S vulkan-mesa-layers
pacman -S vulkan-broadcom
pacman -S vulkan-swrast
```

## Part resize

Check present allocation:

```
tune2fs -l /dev/mmcblk0p3
```

Get new size 46000 Mib in 4k blocks:

```
echo $(( 46000 * 512 ))
```

Use resizefs to scale filesystem:

```
fsck.ext4 -f /dev/mmcblk0p3
resize2fs /dev/mmcblk0p3 23552000
```

Calculate new end (parted to get present start):

```
parted 
  unit Mib
  print   
echo $(( 12489 + 46000 ))
```

Put new end:

```
parted 
  unit Mib
  resizepart 3 
  58489Mib
```

And a final check (detects partition/fs missmatch):

```
fsck.ext4 -f /dev/mmcblk0p3
