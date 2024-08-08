# Programming examples

## Meaning of life

```
import java.math.BigDecimal;
import java.math.BigInteger;
import java.math.MathContext;
import java.util.Random;
// meaning of life as some people will unterstand it
public class MeaningOfLife {
  // Very imported control thread that indispensable
  public static class ControlThread extends Thread {
     private WorkThread workThread;
     private Random rand = new Random(); 
     public ControlThread(WorkThread workThread) {
        super(WorkThread.class.getSimpleName());
        setDaemon(true);
        this.workThread = workThread;
     }
     @Override
     public void run() {
        while(true) {
           try {
              Thread.sleep(rand.nextInt(10));
           }
           catch (InterruptedException exc) {
              exc.printStackTrace();
           }
           workThread.interrupt();
        }
     }
  }
  // Work on some nice things in life like the golden ratio
  public static class WorkThread extends Thread {
     private BigInteger a;
     private BigInteger b;
     private int i;
     public static final int MAX = 1000;
     public WorkThread() {
        init();
     }
     private void init() {
        a = BigInteger.ZERO;
        b = BigInteger.ONE;
        i = 0;
     }      
     @Override
     public void run() {
        while (i < MAX) {
           BigInteger c = a.add(b);
           a = b;
           b = c;
           ++i;
           try {
              // allow some other processing
              Thread.sleep(10l);
           }
           catch (InterruptedException exc) {
              System.out.format("Worker interruped reinit.\n");
              init();
           }
        }
        System.out.format("a: %d\n", a);
        System.out.format("b: %d\n", b);
        System.out.format("phi: %s\n", new BigDecimal(b).divide(new BigDecimal(a), new MathContext(MAX)));
     }
  }   
  public static void main(String[] args) {
     WorkThread workThread = new WorkThread();
     workThread.start();
     ControlThread controlThread = new ControlThread(workThread);
     controlThread.start();
     try {
     workThread.join();      // wait for work thread to complete
     }
     catch (InterruptedException exc) {
        exc.printStackTrace();
     }
  }
}
