# Java

## BackView

![BackView](/images/BackView.png)

If you think about taking backups it might be attractive to use hardlinks see rsync or cp -al. A basic overview is possible with du -hs as it shows the amount of space taken by a backup (it counts the inodes in selection only once). But to allow a bit more insight, the following Java-Program might be helpful to see what was backed up when:

[Media:BackView.zip](Media:BackView.zip.md)

## Java sources

Simple method to check variation of directories using hard-links: 

```
   public int[] test(File dir, Map<Object, String> inodes) throws IOException {
       int add = 0,del = 0;
       File[] files = dir.listFiles();
       Map<Object, String> inodesLocal = new HashMap<>();
       for (File file : files) {
           Path path = file.toPath();
           PosixFileAttributes attr = Files.readAttributes(path, PosixFileAttributes.class);
           String prev = inodes.get(attr.fileKey());
           if (prev == null) {
               add++;
           }
           inodesLocal.put(attr.fileKey(), file.getAbsolutePath());
       }
       inodes.keySet().removeAll(inodesLocal.keySet());
       del = inodes.size();
       inodes.clear();
       inodes.putAll(inodesLocal);
       return new int[] {add, del};
   }
```

To access the inode value directly one may use:

```
Map<String, Object> attrs = Files.readAttributes(path, "unix:*");   
```

and check the value "ino".

## Covid

If you are looking for some informations about the digital Eu-certificat (Qr-code) the [Android App repository](https://github.com/Digitaler-Impfnachweis/covpass-android) might be helpful.

Information on digital-certificat RKI [impf-app](https://digitaler-impfnachweis-app.de/technische-details/).

The [rfc8152](https://datatracker.ietf.org/doc/rfc8152/) (abstract) definition of the certificat format (COSE).

Or if you just want to have a look on some simple Java example on a vaccination use the following Maven/(Netbeans) project (with CertInfo input_image).
Requires a scanned Qr-code image, allows rewriting the Qr-Code-Copy as backup (with CertInfo input_image output.png)... the testing/recovery information shoud be easy to add, but i had non of these available. Used [java-green pass](https://gae-piaz.medium.com/decode-the-eu-green-pass-qrcode-using-java-b5654e55b0fc) as a starting point.

[Media:Corvid.zip](Media:Corvid.zip.md)

## Lexical sorting

My suggestion for a lexical sorting extended from (which implements DIN 5007):

[Medium.org/fme](https://medium.com/fme-developer-stories/how-to-sort-umlaute-in-java-correctly-13f3262f15a1)

```
   private static void sort(List<String> strs, Collator coll) {
       List<String> cstrs = new ArrayList<>(strs);
       Collections.sort(cstrs, coll);        
       String prev = "";
       for (String str : cstrs) {
           System.out.format("  %s %d\n", str, coll.compare(str, prev));
           prev = str;
       }        
   }
       private static final String EXT_RULES = "< ' ' < '.'"+
 "<0<1<2<3<4<5<6<7<8<9<a & ae,ä,A & AE,Ä<b,B<c,C<d,D<ð,Ð<e,E<f,F<g,G<h,H<i,I<j"+
 ",J<k,K<l,L<m,M<n,N<o & oe,ö,O & OE,Ö<p,P<q,Q<r,R<s, S & SS,ß<t,T& TH, Þ &TH,"+
 "þ <u & ue,ü,U & UE,Ü<v,V<w,W<x,X<y,Y<z,Z&AE,Æ&AE,æ&OE,Œ&OE,œ";    
   public static void main(String[] args) throws Exception {
       List<String> strs = new ArrayList<>();
       RuleBasedCollator deCol = (RuleBasedCollator)Collator.getInstance(Locale.GERMAN);
       Collator col = new RuleBasedCollator(deCol.getRules() + EXT_RULES);
       col.setStrength(Collator.PRIMARY);
       strs.add("haber");
       strs.add("Hafen");
       strs.add("Härten");
       strs.add("Härte");
       strs.add("Haerte");
       strs.add("Haber");        
       strs.add("Mohshärte");
       strs.add("Mörtel");
       strs.add("Modulation");   
       sort(strs, col);
```

Result (the number indicates if it is considered less/greater or equal to previous value):

```
 haber 1
 Haber 0
 Härte 1
 Haerte 0
 Härten 1
 Hafen 1
 Modulation 1
 Mörtel 1
 Mohshärte 1
```

## Alternative Jdk source

[Available as eclipse temurin](https://adoptium.net/releases.html?variant=openjdk8&jvmVariant=hotspot)

## Bedwork

Use java8

```
git clone https://github.com/Bedework/bw-caldav.git --branch bw-caldav-4.0.7
```

Interface to implement backend:

```
org.bedework.caldav.server.sysinterface.SysIntf
```

Servlet template:

```
bw-calendar-engine-ucaldav
```

Default impl override by out impl?

```
bw-calendar-engine-impl
```


| Level  |Modul
| --- | --- |
|0  | bw-util-loggin-5.01 |
|1  | bw-util-misc-4.0-32 |
|2  | |

Maven install locally:

```
cd /home/rpf/bedework/bw-util2/bw-util2-calendar; JAVA_HOME=/usr/lib/jvm/default /usr/local/netbeans/java/maven/bin/mvn -DartifactId=bw-xml-icalendar -DgroupId=org.bedework -Dversion=4.0.10 -Dpackaging=jar -Dfile=/home/rpf/bedework/bw-xml/bw-xml-icalendar/target/bw-xml-icalendar-4.0.12-SNAPSHOT.jar -DgeneratePom=false install:install-file
```

This is something for the academic interested.

## Cosmo

Some workable solution for Java8 (no CardDav).

https://github.com/1and1/cosmo

## Crypto with AES

As it seems the compatibility of open-ssl and Java is no that easy here a example:

```
/*
* Copyright (C) 2022 rpf
*
* This program is free software: you can redistribute it and/or modify
* it under the terms of the GNU General Public License as published by
* the Free Software Foundation, either version 3 of the License, or
* (at your option) any later version.
*
* This program is distributed in the hope that it will be useful,
* but WITHOUT ANY WARRANTY; without even the implied warranty of
* MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
* GNU General Public License for more details.
*
* You should have received a copy of the GNU General Public License
* along with this program.  If not, see <http://www.gnu.org/licenses/>.
*/
package de.pfeifer_syscon.work.codeAES;
import java.nio.charset.Charset;
import java.security.SecureRandom;
import java.security.spec.KeySpec;
import java.util.Base64;
import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;
import javax.crypto.spec.GCMParameterSpec;
import javax.crypto.spec.PBEKeySpec;
import javax.crypto.spec.SecretKeySpec;
/**
* a openssl compatible AES en-/decyption
*
* @author rpf
*/
public class Crypt {
   public byte[] decodeBase64(String src) throws Exception {
       Base64.Decoder dec = Base64.getDecoder();
       byte[] decoded = dec.decode(src);
       return decoded;
   }
   public String encodeBase64(byte[] src) throws Exception {
       Base64.Encoder enc = Base64.getEncoder();
       byte[] encoded = enc.encode(src);
       return new String(encoded, Charset.forName("ASCII"));
   }
   public byte[] decrypt(byte[] cipherText, String add, SecretKey key, byte[] iv, byte[] tag) throws Exception {
       // Get Cipher Instance
       Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding");
       // Create GCMParameterSpec
       GCMParameterSpec gcmParameterSpec = new GCMParameterSpec(tag.length * 8, iv);
       // Initialize Cipher for DECRYPT_MODE
       cipher.init(Cipher.DECRYPT_MODE, key, gcmParameterSpec);
       // additional 
       cipher.updateAAD(add.getBytes(Charset.forName("UTF-8")));
       // Perform Decryption
       cipher.update(cipherText);
       byte[] decrypted = cipher.doFinal(tag); // the tag is required on be in final
       return decrypted;
   }
   public byte[] encrypt(byte[] plainText, String add, SecretKey key, byte[] iv, byte[] tag) throws Exception {
       // Get Cipher Instance
       Cipher cipher = Cipher.getInstance("AES/GCM/NoPadding");
       // Create GCMParameterSpec
       GCMParameterSpec gcmParameterSpec = new GCMParameterSpec(tag.length * 8, iv);
       // Initialize Cipher for ENCRYPT_MODE
       cipher.init(Cipher.ENCRYPT_MODE, key, gcmParameterSpec);
       // additional
       cipher.updateAAD(add.getBytes(Charset.forName("UTF-8")));
       // Perform encryption
       byte[] enc = cipher.update(plainText);                  // any full block will be returned here
       byte[] realEnc = new byte[plainText.length];            // assumption encoded has same length (may change if algo/padding changes)
       if (enc != null) {
           System.arraycopy(enc, 0, realEnc, 0, enc.length);   // use as much as we get here
       }
       byte[] remain = cipher.doFinal();                       // here the last incomplete block and the tag is returned
       System.arraycopy(remain, 0, realEnc, enc != null ? enc.length : 0, remain.length - tag.length);
       System.arraycopy(remain, remain.length - tag.length, tag, 0, tag.length); // and return tag of the requested size
       return realEnc;
   }
   private SecretKey getSecret(String passphrase, byte[] saltBytes, int iter) throws Exception {
       SecretKeyFactory secFactory = SecretKeyFactory.getInstance("PBKDF2WithHmacSHA256");
       KeySpec spec = new PBEKeySpec(passphrase.toCharArray(), saltBytes, iter, 256);
       return new SecretKeySpec(secFactory.generateSecret(spec).getEncoded(), "AES");
   }
   /*
    * Example:
    * Salt ICAYGYLMoJg7V1BQDdEYlw== keyIter 1024
    * Encrypted dxy/6lf5lXnv2/kL4QLcha82ZOtzhUmPHKUvlk+Tfe5+y2nwNjDCe4YYZBYWiFuLI7Ctp21cQ9neF+N37HI1B3iGEf08Ag== tag sGayvykFzE/IpQPgaOb0ZA==
    * Decrypted abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789öäüß
    */
   public static void main(String[] args) {
       try {
           Crypt crypt = new Crypt();
           String passphrase = "passphraseÖÄÜ";
           byte[] salt = new byte[16];
           SecureRandom random = new SecureRandom();
           random.nextBytes(salt);
           int keyIter = 1024;
           SecretKey secretKey = crypt.getSecret(passphrase, salt, keyIter);
           System.out.format("Salt %s keyIter %d\n", crypt.encodeBase64(salt), keyIter);
           String plain = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789öäüß";
           String add = "name";    // ensures that a encryption for a diffrent context may not be exchanged
           byte[] iv = new byte[12];
           random.nextBytes(iv);
           byte[] tag = new byte[16];
           byte[] encrypt = crypt.encrypt(plain.getBytes(Charset.forName("UTF-8")), add, secretKey, iv, tag);
           System.out.format("Encrypted %s tag %s\n", crypt.encodeBase64(encrypt), crypt.encodeBase64(tag));
           byte[] decrypt = crypt.decrypt(encrypt, add, secretKey, iv, tag);
           String text = new String(decrypt, Charset.forName("UTF-8"));
           System.out.format("Decrypted %s \n", text);
       }
       catch (Exception exc) {
           exc.printStackTrace();
       }
   }
}
```

### And now c++ header

```
/*
* Copyright (C) 2022 rpf
*
* This program is free software: you can redistribute it and/or modify
* it under the terms of the GNU General Public License as published by
* the Free Software Foundation, either version 3 of the License, or
* (at your option) any later version.
*
* This program is distributed in the hope that it will be useful,
* but WITHOUT ANY WARRANTY; without even the implied warranty of
* MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
* GNU General Public License for more details.
*
* You should have received a copy of the GNU General Public License
* along with this program.  If not, see <http://www.gnu.org/licenses/>.
*/
#pragma once
#include <string>
#include <memory>
#include <openssl/evp.h>
class CryptException : public std::exception {
public:
   CryptException(const std::string &what);
   virtual ~CryptException();
   virtual const char* what() const throw();
private:
   std::string m_what;
};
class CryptBuf {
public:
   CryptBuf();
   CryptBuf(size_t size);
   CryptBuf(const std::string& base64);
   virtual ~CryptBuf();
   CryptBuf& operator=(const CryptBuf& other);
   void setSize(size_t size);
   size_t getSize() const;
   unsigned char *getBuf() const;
   void rand();
   void dump();
   std::string base64();
private:
   size_t m_size;
   unsigned char *m_buf;
};
class CryptHmac {
public:
   CryptHmac(const std::string& secret, size_t iter);
   CryptHmac(const std::string& secret, const std::string& saltBase64, size_t iter);
   virtual ~CryptHmac();
   std::shared_ptr<CryptBuf> getKey() const;
   std::shared_ptr<CryptBuf> getSalt() const;
   size_t getIterations() const;
protected:
   CryptHmac(size_t iter);
   void generateKey(const std::string& secret);
private:
   const EVP_MD *m_mdigest;
   size_t m_iterations;
   std::shared_ptr<CryptBuf> m_key;
   std::shared_ptr<CryptBuf> m_salt;
};
class CryptMessage {
public:
   CryptMessage(const std::string& message, const std::string& additional, const CryptHmac& cryptHmac);
   CryptMessage(const std::string& ivBase64, const std::string& cipherTextBase64, const std::string& additional, const CryptHmac& cryptHmac, const std::string& tagBase64);
   virtual ~CryptMessage();
   int encrypt(unsigned char *plaintext, int plaintext_len,
               unsigned char *aad, int aad_len,
               unsigned char *ciphertext);
   int decrypt(unsigned char *ciphertext, int ciphertext_len,
               unsigned char *aad, int aad_len,
               unsigned char *plaintext);
   int randIV();
   int setIV(const unsigned char *iv);
   const std::shared_ptr<CryptBuf> getIV();
   const std::shared_ptr<CryptBuf> getCipherText();
   const std::string getPlainText();
   const std::shared_ptr<CryptBuf> getTag();
protected:
   CryptMessage();
private:
   void handleErrors(void);
   std::shared_ptr<CryptBuf> m_key;
   std::shared_ptr<CryptBuf> m_iv;
   std::shared_ptr<CryptBuf> m_tag;
   const EVP_CIPHER *m_cipher;
   std::shared_ptr<CryptBuf> m_ciphertext;
   std::shared_ptr<CryptBuf> m_plaintext;
};
```

### c++ source

```

 /*
 * Copyright (C) 2022 rpf
 *
 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program.  If not, see <http://www.gnu.org/licenses/>.
 */
 #include <glibmm.h>
 #include <openssl/conf.h>
 #include <openssl/err.h>
 #include <openssl/rand.h>
 #include <stdio.h>
 #include <iostream>
 #include <cstring>
 #include "Crypt.hpp"
 CryptException::CryptException(const std::string &what)
 : m_what{what}
 {
 }
 CryptException::~CryptException()
 {
 }
 const char*
 CryptException::what() const throw()
 {
     return m_what.c_str();
 }
 CryptBuf::CryptBuf()
 : m_size(0u)
 , m_buf{nullptr}
 {
 }
 CryptBuf::CryptBuf(size_t size)
 : CryptBuf()
 {
    setSize(size);
 }
 CryptBuf::CryptBuf(const std::string& base64)
 : CryptBuf()
 {
    int size = base64.length() / 4 * 3;
    setSize(size);

    int ret = EVP_DecodeBlock(m_buf, (const unsigned char *)base64.c_str(), base64.length());
    if (ret < 0) {
        throw CryptException(Glib::ustring::sprintf("Error EVP_DecodeBlock size %d\n", size));
    }
    // remove padding
    if (base64.length() > 2 && base64.substr(base64.length()-2) == "==") {
        m_size -= 2;
    }
    else if (base64.length() > 1 && base64.substr(base64.length()-1) == "=") {
        m_size--;
    }
 }
 CryptBuf&
 CryptBuf::operator=(const CryptBuf& other)
 {
    setSize(other.getSize());
    memcpy(m_buf, other.getBuf(), other.getSize());
    return *this;
 }
 CryptBuf::~CryptBuf()
 {
    if (m_buf) {
        delete[] m_buf;
        m_buf = nullptr;
    }
 }
 void
 CryptBuf::setSize(size_t size)
 {
    if (size != m_size) {
        if (m_buf) {
            delete[] m_buf;
            m_buf = nullptr;
        }
        if (size > 0) {
            m_buf = new unsigned char[size];
        }
        m_size = size;
    }
 }
 size_t
 CryptBuf::getSize() const
 {
    return m_size;
 }
 unsigned char *
 CryptBuf::getBuf() const
 {
    return m_buf;
 }
 void
 CryptBuf::rand()
 {
    if (!m_buf) {
        throw CryptException(Glib::ustring::sprintf("Error gen rand no buf size %d\n", m_size));
    }
    if (1 != RAND_bytes(m_buf, m_size)) {
        throw CryptException(Glib::ustring::sprintf("Error gen rand %d\n", m_size));
    }
 }
 void
 CryptBuf::dump()
 {
    BIO_dump_fp(stdout, (const char *)m_buf, m_size);
 }
 std::string
 CryptBuf::base64()
 {
    if (!m_buf) {
        throw CryptException(Glib::ustring::sprintf("Error gen rand no buf size %d\n", m_size));
    }
    uint8_t* base64 = new uint8_t[(m_size / 3 + 1) * 4 + 1];    // 3bytes eventually padded will result in 4bytes base64 plus closing '\0'
    if (!base64) {
        throw CryptException(Glib::ustring::sprintf("Error alloc base64 buf size %d\n", m_size));
    }
    EVP_EncodeBlock(base64, m_buf, m_size);
    std::string data((const char *)base64);
    delete[] base64;
    return data;
 }
 CryptHmac::CryptHmac(size_t iter)
 : m_mdigest{EVP_sha256()}
 , m_iterations{iter}
 , m_key{std::make_shared<CryptBuf>(32u)}
 , m_salt{std::make_shared<CryptBuf>(32u)}
 {
 }
 CryptHmac::CryptHmac(const std::string& secret, size_t iter)
 : CryptHmac{iter}
 {
    m_salt->rand();
    generateKey(secret);
 }
 CryptHmac::CryptHmac(const std::string& secret, const std::string& saltBase64, size_t iter)
 : CryptHmac{iter}
 {
    m_salt = std::make_shared<CryptBuf>(saltBase64);
    generateKey(secret);
 }
 CryptHmac::~CryptHmac()
 { 

 }
 size_t
 CryptHmac::getIterations() const
 {
    return m_iterations;
 }
 void
 CryptHmac::generateKey(const std::string& secret)
 {
    const char* cpass = (const char*)secret.c_str();
    size_t pass_len = strlen(cpass);
    int ret = PKCS5_PBKDF2_HMAC(cpass, pass_len, m_salt->getBuf(), m_salt->getSize(), m_iterations, m_mdigest, m_key->getSize(), m_key->getBuf());
    if (1 != ret) {
        throw CryptException(Glib::ustring::sprintf("PKCS5_PBKDF2_HMAC failed, error 0x%lx\n", ERR_get_error()));
    }
 }
 std::shared_ptr<CryptBuf> 
 CryptHmac::getKey() const
 {
    return m_key;
 }
 std::shared_ptr<CryptBuf> 
 CryptHmac::getSalt() const
 {
    return m_salt;
 }
 CryptMessage::CryptMessage()
 : m_key{std::make_shared<CryptBuf>(32u)}
 // the recommendation is to use 12bytes as additional seems not to improve anything
 // https://crypto.stackexchange.com/questions/26783/ciphertext-and-tag-size-and-iv-transmission-with-aes-in-gcm-mode/26787#26787?newreg=3942556a7f664d16a466b114e0fc993e
 , m_iv{std::make_shared<CryptBuf>(12u)}
 , m_tag{std::make_shared<CryptBuf>(16u)}
 , m_cipher{EVP_aes_256_gcm()}
 , m_ciphertext{}
 , m_plaintext{}
 {
 }
 CryptMessage::CryptMessage(const std::string& message, const std::string& additional,
                            const CryptHmac& cryptHmac)
 : CryptMessage{}
 {
    m_key = cryptHmac.getKey();
    m_iv->rand();
    /*
     * Buffer for ciphertext. Ensure the buffer is long enough for the
     * ciphertext which may be longer than the plaintext, depending on the
     * algorithm and mode.
     */
    m_ciphertext = std::make_shared<CryptBuf>(message.length());    // for aes this shoud equal the input size
    /* Encrypt the plaintext */
    int ciphertext_len = encrypt((uint8_t *)message.c_str(), message.length(),
                                 (uint8_t *)additional.c_str(), additional.length(),
                                 m_ciphertext->getBuf());
    if (message.length() != ciphertext_len) {
        fprintf(stderr, "Unexpected length cipher %d expected %d\n", ciphertext_len, message.length() );
    }
    /* Do something useful with the ciphertext here */
    //printf("Ciphertext is:\n");
    //m_ciphertext->dump();
    //printf("Tag is:\n");
    //m_tag->dump();
 }
 CryptMessage::CryptMessage(const std::string& ivBase64, const std::string& cipherTextBase64,
                            const std::string& additional, const CryptHmac& cryptHmac,
                            const std::string& tagBase64)
 : CryptMessage()
 {
    m_key = cryptHmac.getKey();
    m_iv = std::make_shared<CryptBuf>(ivBase64);
    //printf("iv is:\n");
    //m_iv->dump();
    m_tag = std::make_shared<CryptBuf>(tagBase64);
    m_ciphertext = std::make_shared<CryptBuf>(cipherTextBase64);
    m_plaintext = std::make_shared<CryptBuf>(m_ciphertext->getSize() + 1);
    /* Decrypt the ciphertext */
    int decryptedtext_len = decrypt(m_ciphertext->getBuf(), m_ciphertext->getSize(),
                                (uint8_t *)additional.c_str(), additional.length(),
                                m_plaintext->getBuf());
    if (decryptedtext_len >= 0) {
        /* Add a NULL terminator. We are expecting printable text */
        m_plaintext->getBuf()[decryptedtext_len] = '\0';
        /* Show the decrypted text */
        //printf("Decrypted text is:\n");
        //printf("%s\n", m_plaintext->getBuf());
    } else {
        printf("Decryption failed\n");
    }
 }
 CryptMessage::~CryptMessage()
 {
 }
 void
 CryptMessage::handleErrors(void)
 {
    ERR_print_errors_fp(stderr);
 }
 int
 CryptMessage::setIV(const unsigned char *iv)
 {
    memcpy(m_iv->getBuf(), iv, m_iv->getSize());
    return 1;
 }
 const std::shared_ptr<CryptBuf>
 CryptMessage::getIV()
 {
    return m_iv;
 }
 const std::shared_ptr<CryptBuf>
 CryptMessage::getCipherText()
 {
    return m_ciphertext;
 }
 const std::string
 CryptMessage::getPlainText()
 {
    std::string text((const char*)m_plaintext->getBuf());
    return text;
 }
 const std::shared_ptr<CryptBuf>
 CryptMessage::getTag()
 {
    return m_tag;
 }
 int
 CryptMessage::encrypt(unsigned char *plaintext, int plaintext_len,
                unsigned char *aad, int aad_len,
                unsigned char *ciphertext)
 {
    EVP_CIPHER_CTX *ctx;
    /* Create and initialise the context */
    if(!(ctx = EVP_CIPHER_CTX_new())) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_CIPHER_CTX_new %d", ERR_get_error()));
    }
    /* Initialise the encryption operation. */
    if(1 != EVP_EncryptInit_ex(ctx, m_cipher, NULL, NULL, NULL)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_EncryptInit_ex %d", ERR_get_error()));
    }
    /*
     * Set IV length if default 12 bytes (96 bits) is not appropriate
     *   so raised this value from 128 to 256
     */
    if(1 != EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_IVLEN, m_iv->getSize(), NULL)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_EncryptInit_ex size %d", m_iv->getSize()));
    }
    /* Initialise key and IV */
    if(1 != EVP_EncryptInit_ex(ctx, NULL, NULL, m_key->getBuf(), m_iv->getBuf())) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_EncryptInit_ex %d", ERR_get_error()));
    }
    /*
     * Provide any AAD data. This can be called zero or more times as
     * required
     */
    int len;
    if(1 != EVP_EncryptUpdate(ctx, NULL, &len, aad, aad_len)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_EncryptUpdate add len %d", aad_len));
    }
    /*
     * Provide the message to be encrypted, and obtain the encrypted output.
     * EVP_EncryptUpdate can be called multiple times if necessary
     */
    if(1 != EVP_EncryptUpdate(ctx, ciphertext, &len, plaintext, plaintext_len)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_EncryptUpdate plain len %d", plaintext_len));
    }
    int ciphertext_len = len;
    /*
     * Finalise the encryption. Normally ciphertext bytes may be written at
     * this stage, but this does not occur in GCM mode
     */
    if(1 != EVP_EncryptFinal_ex(ctx, ciphertext + len, &len)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_EncryptFinal_ex %d", ERR_get_error()));
    }
    ciphertext_len += len;
    /* Get the tag */
    if(1 != EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_GET_TAG, m_tag->getSize(), m_tag->getBuf())) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_CIPHER_CTX_ctrl tag len %d", m_tag->getSize()));
    }
    /* Clean up */
    EVP_CIPHER_CTX_free(ctx);
    return ciphertext_len;
 }
 int
 CryptMessage::decrypt(unsigned char *ciphertext, int ciphertext_len,
                unsigned char *aad, int aad_len,
                unsigned char *plaintext)
 {
    EVP_CIPHER_CTX *ctx;
    /* Create and initialise the context */
    if(!(ctx = EVP_CIPHER_CTX_new())) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_CIPHER_CTX_new %d", ERR_get_error()));
    }
    /* Initialise the decryption operation. */
    if(!EVP_DecryptInit_ex(ctx, m_cipher, NULL, NULL, NULL)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_DecryptInit_ex %d", ERR_get_error()));
    }
    /* Set IV length. Not necessary if this is 12 bytes (96 bits) */
    if(!EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_IVLEN, m_iv->getSize(), NULL)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_CIPHER_CTX_ctrl ivlen %d", m_iv->getSize()));
    }
    /* Initialise key and IV */
    if(!EVP_DecryptInit_ex(ctx, NULL, NULL, m_key->getBuf(), m_iv->getBuf())) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_DecryptInit_ex %d", ERR_get_error()));
    }
    /*
     * Provide any AAD data. This can be called zero or more times as
     * required
     * this provides some context, and prevents the message to be
     *    decoded from a different context
     */
    int len;
    if(!EVP_DecryptUpdate(ctx, NULL, &len, aad, aad_len)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_DecryptUpdate add len %d", aad_len));
    }
    /*
     * Provide the message to be decrypted, and obtain the plaintext output.
     * EVP_DecryptUpdate can be called multiple times if necessary
     */
    if(!EVP_DecryptUpdate(ctx, plaintext, &len, ciphertext, ciphertext_len)) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_DecryptUpdate cipher len %d", ciphertext_len));
    }
    int plaintext_len = len;
    /* Set expected tag value. Works in OpenSSL 1.0.1d and later */
    if(!EVP_CIPHER_CTX_ctrl(ctx, EVP_CTRL_GCM_SET_TAG, m_tag->getSize(), m_tag->getBuf())) {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_CIPHER_CTX_ctrl tag len %d", m_tag->getSize()));
    }
    /*
     * Finalise the decryption. A positive return value indicates success,
     * anything else is a failure - the plaintext is not trustworthy.
     */
    int ret = EVP_DecryptFinal_ex(ctx, plaintext + len, &len);
    /* Clean up */
    EVP_CIPHER_CTX_free(ctx);
    if(ret > 0) {
        /* Success */
        plaintext_len += len;
        return plaintext_len;
    } else {
        handleErrors();
        throw CryptException(Glib::ustring::sprintf("Error EVP_DecryptFinal_ex len %d", len));
    }
 }
 /*
 * Example
 * Salt T7oYH7CnLegoPK/quz9QiQ== keyIter 1024
 * Encrypted bGD1NlfxA5+tzbSUHIMF9lmNBQcxcBuD3cS44CAn/x0uzQoq0IRSYz/svOzLlv2yU69uZ4gKEBsirrHNKt0Go4P7FH01cw== tag nUUTAs/aB8D3NsKSlJZ5og==
 * Decrypted abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789öäüß 
 */
 int main(int argc, char** argv)
 {
        try {
            Glib::ustring passphrase(u8"passphraseÖÄÜ");
            auto salt = std::make_shared<CryptBuf>(16u);
            salt->rand();
            size_t keyIter = 1024;
            CryptHmac secretKey(passphrase, salt->base64(), keyIter);
            printf("Salt %s keyIter %d\n", salt->base64().c_str(), keyIter);
            Glib::ustring plain = u8"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789öäüß";
            Glib::ustring add = u8"name";    // ensures that a encryption for a different context may not be exchanged
            CryptMessage encrypt(plain, add, secretKey);
            auto iv = encrypt.getIV();
            auto tag = encrypt.getTag();
            auto encrypted = encrypt.getCipherText();
            printf("Encrypted %s tag %s\n", encrypted->base64().c_str(), tag->base64().c_str());
            CryptMessage decrypt(iv->base64(), encrypted->base64(), add, secretKey, tag->base64());
            auto text = decrypt.getPlainText();
            printf("Decrypted %s \n", text.c_str());
        }
        catch (const CryptException& exc) {
            std::cout << "Exception " << exc.what() << std::endl;
        }
 }
```

## Custom painting

```

 package de.pfeifersyscon.charfill;

 import java.awt.BorderLayout;
 import java.awt.Color;
 import java.awt.Dimension;
 import java.awt.Font;
 import java.awt.FontMetrics;
 import java.awt.GradientPaint;
 import java.awt.Graphics;
 import java.awt.Graphics2D;
 import java.awt.Rectangle;
 import java.awt.RenderingHints;
 import java.awt.Shape;
 import java.awt.font.FontRenderContext;
 import java.awt.font.GlyphVector;
 import java.awt.geom.AffineTransform;
 import javax.swing.JFrame;
 import javax.swing.JPanel;
 import javax.swing.SwingUtilities;

 /**
 *
 * @author rpf
 */
 public class Charfill extends JFrame {

    public class Drawingarea extends JPanel {

        protected Drawingarea() {
            Dimension dim = new Dimension(640, 480);
            setPreferredSize(dim);
            setMinimumSize(dim);
        }

        public void paintComponent(Graphics g) {
            super.paintComponent(g);

            Graphics2D g2 = (Graphics2D)g;
            g2.setRenderingHint(RenderingHints.KEY_TEXT_ANTIALIASING, RenderingHints.VALUE_TEXT_ANTIALIAS_ON);
            Font font = Font.decode("serif-normal-48");
            FontMetrics fontMetrics = getFontMetrics(font);
            String text = "Hello, World!";
            int width = fontMetrics.stringWidth(text);
            int height = fontMetrics.getHeight();

            FontRenderContext frc = fontMetrics.getFontRenderContext();
            GlyphVector v = font.createGlyphVector(frc, text);
            Shape textShape = v.getOutline();
            g2.transform(AffineTransform.getTranslateInstance(10.0, 20.0+height));
            Shape rect = new Rectangle(0,0, width,height);
            GradientPaint paint = new GradientPaint(0,0,Color.RED,width, height,Color.GREEN);
            g2.setPaint(paint);
            g2.fill(textShape);
            //for (int i = 0; i < width+height; ++i) {
            //    g2.setColor(i % 2 == 0 ? Color.GREEN : Color.RED);
            //    g2.drawLine(i, 0, i-height, height);
            //}
            //g2.dispose();
        }

    }

    private Drawingarea drawingarea;

    protected Charfill() {
        super("Charfill");
        setDefaultCloseOperation(EXIT_ON_CLOSE);

        drawingarea = new Drawingarea();
        add(drawingarea, BorderLayout.CENTER);

        pack();
    }


    private static void initGui() {
        SwingUtilities.invokeLater(new Runnable() {
            @Override
            public void run() {
                Charfill charfill = new Charfill();
                charfill.setVisible(true);
            }
        });
    }

    public static void main(String[] args) {
       initGui();
    }
 }
```
