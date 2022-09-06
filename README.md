# Replication Package for REALAU


## Tool Screenshot
We implment a web based tool to demonstrate the recommendation results by our approach.

<object data="http://yoursite.com/the.pdf" type="application/pdf" width="100%">
    <embed src="./tool demo.pdf" />
</object>


We conduct a series of experiments to evaluate the accuracy, usefulness, and quality of REALAU by answering the following research questions.
- **RQ1 (Accuracy for API Usage Prediction):** How accurately REALAU can predict the masked API usages in code?
- **RQ2 (Usefulness for Code Customization):** Are the code alternatives recommended by REALAU useful for code customization tasks?
- **RQ3 (Quality of Recommended Alternatives):** What is the quality of the code alternatives recommended by REALAU?

In the above three research questions, RQ2 and RQ3 involve 6 code customization tasks.
We list the tasks and explain how the questions in the tasks are collected.

### Task 1
Context: How do I use 3DES encryption/decryption in Java?
```java
public byte[] encrypt(String message) throws Exception {
    final MessageDigest md = MessageDigest.getInstance("md5");
    final byte[] digestOfPassword = md.digest("HG58YZ3CR9".getBytes("utf-8"));
    final byte[] keyBytes = Arrays.copyOf(digestOfPassword, 24);
    for (int j = 0, k = 16; j < 8;) {
    keyBytes[k++] = keyBytes[j++];
    }

    final SecretKey key = new SecretKeySpec(keyBytes, "DESede");
    final IvParameterSpec iv = new IvParameterSpec(new byte[8]);
    final Cipher cipher = Cipher.getInstance("DESede/CBC/PKCS5Padding");
    cipher.init(Cipher.ENCRYPT_MODE, key, iv);

    final byte[] plainTextBytes = message.getBytes("utf-8");
    final byte[] cipherText = cipher.doFinal(plainTextBytes);
    // final String encodedCipherText = new sun.misc.BASE64Encoder()
    // .encode(cipherText);

    return cipherText;
}

```
- **Q1:** To solve the securi issue of "MD5" digest algorithm in this code, we can modify the algorithm "MD5" to (please give 3 options):
- **Q2:** If you didn’t know the problem before, do you think the results recommend by the tool help you to realize the problem? (answer yes/no)

The questions are derived from the following SO posts and comments:
- https://stackoverflow.com/questions/73327158/checksum-generation
- https://stackoverflow.com/questions/35853757/javax-crypto-badpaddingexception-given-final-block-not-properly-padded-tried
- https://stackoverflow.com/questions/12642742/exception-when-calling-messagedigest-getinstancesha256
- https://stackoverflow.com/questions/304268/getting-a-files-md5-checksum-in-java/304350#304350

### Task 2
Context: Generate an ASCII picture from a string
```java
BufferedImage image = new BufferedImage(144, 32, BufferedImage.TYPE_INT_RGB);
Graphics g = image.getGraphics();
g.setFont(new Font("Dialog", Font.PLAIN, 24));
Graphics2D graphics = (Graphics2D) g;
graphics.setRenderingHint(RenderingHints.KEY_TEXT_ANTIALIASING,
        RenderingHints.VALUE_TEXT_ANTIALIAS_ON);
graphics.drawString("Hello World!", 6, 24);
ImageIO.write(image, "png", new File("text.png"));

for (int y = 0; y < 32; y++) {
    StringBuilder sb = new StringBuilder();
    for (int x = 0; x < 144; x++)
        sb.append(image.getRGB(x, y) == -16777216 ? " " : image.getRGB(x, y) == -1 ? "#" : "*");
    if (sb.toString().trim().isEmpty()) continue;
    System.out.println(sb);
}
```
- **Q1:** To use different fonts, we can modify the font "Dialog" to (please give 5 options):
- **Q2:** If you didn’t know the problem before, do you think the results recommend by the tool help you to realize the problem? (answer yes/no)

The questions are derived from the following SO posts and comments:
- https://stackoverflow.com/questions/28853176/painting-japanese-characters-using-arial-fonts-with-method-drawstring-graph


### Task 3
Context: Colorize a picture in java
```java
private static BufferedImage dye(BufferedImage image, Color color) {
    int w = image.getWidth();
    int h = image.getHeight();
    BufferedImage dyed = new BufferedImage(w,h,BufferedImage.TYPE_INT_ARGB);
    Graphics2D g = dyed.createGraphics();
    g.drawImage(image, 0,0, null);
    g.setComposite(AlphaComposite.SrcAtop);
    g.setColor(color);
    g.fillRect(0,0,w,h);
    g.dispose();
    return dyed;
}
```
- **Q1:** To use different types of images, we can modify the type "BufferedImage.TYPE_INT_ARGB" to (please give 3 options):
- **Q2:** If you didn’t know the problem before, do you think the results recommend by the tool help you to realize the problem? (answer yes/no)

The questions are derived from the following SO posts and comments:
- https://stackoverflow.com/questions/13605248/java-converting-image-to-bufferedimage
- https://stackoverflow.com/questions/9089675/creating-huge-bufferedimage

### Task 4
Context: How to fix the "java.security.cert.CertificateException: No subject alternative names present" error?
```java
private static void disableSslVerification() {
    try
    {
        // Create a trust manager that does not validate certificate chains
        TrustManager[] trustAllCerts = new TrustManager[] {new X509TrustManager() {
            public java.security.cert.X509Certificate[] getAcceptedIssuers() {
                return null;
            }
            public void checkClientTrusted(X509Certificate[] certs, String authType) {
            }
            public void checkServerTrusted(X509Certificate[] certs, String authType) {
            }
        }
        };
        // Install the all-trusting trust manager
        SSLContext sc = SSLContext.getInstance("SSL");
        sc.init(null, trustAllCerts, new java.security.SecureRandom());
        HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());
        // Create all-trusting host name verifier
        HostnameVerifier allHostsValid = new HostnameVerifier() {
            public boolean verify(String hostname, SSLSession session) {
                return true;
            }
        };
        // Install the all-trusting host verifier
        HttpsURLConnection.setDefaultHostnameVerifier(allHostsValid);
    } catch (NoSuchAlgorithmException e) {
        e.printStackTrace();
    } catch (KeyManagementException e) {
        e.printStackTrace();
    }
}
```
- **Q1:** To use different ssl contexts, we can modify the "SSL" to (please give 3 options):
- **Q2:** If you didn’t know the problem before, do you think the results recommend by the tool help you to realize the problem? (answer yes/no)

The questions are derived from the following SO posts and comments:
- https://stackoverflow.com/questions/41241493/java-httpsserver-class-support-for-multiple-protocols

### Task 5
Context: Sockets: Discover port availability using Java
```java
private static boolean available(int port) {
    System.out.println("--------------Testing port " + port);
    Socket s = null;
    try {
        s = new Socket("localhost", port);

        // If the code makes it this far without an exception it means
        // something is using the port and has responded.
        System.out.println("--------------Port " + port + " is not available");
        return false;
    } catch (IOException e) {
        System.out.println("--------------Port " + port + " is available");
        return true;
    } finally {
        if( s != null){
            try {
                s.close();
            } catch (IOException e) {
                throw new RuntimeException("You should handle this error." , e);
            }
        }
    }
}
```
- **Q1:** To use different network protocols, we can modify the API "Socket" to :
- **Q2:** If you didn’t know the problem before, do you think the results recommend by the tool help you to realize the problem? (answer yes/no)

The questions are derived from the following SO posts and comments:
- https://stackoverflow.com/questions/42394386/socket-new-socketserveraddr-port


### Task 6
Context: Read/Write String from/to a File in Android
```java
private String readFromFile(Context context) {
    String ret = "";
    try {
        InputStream inputStream = context.openFileInput("config.txt");
        if ( inputStream != null ) {
            InputStreamReader inputStreamReader = new InputStreamReader(inputStream);
            BufferedReader bufferedReader = new BufferedReader(inputStreamReader);
            String receiveString = "";
            StringBuilder stringBuilder = new StringBuilder();
            while ( (receiveString = bufferedReader.readLine()) != null ) {
                stringBuilder.append(receiveString);
            }
            inputStream.close();
            ret = stringBuilder.toString();
        }
    }
    catch (FileNotFoundException e) {
        Log.e("login activity", "File not found: " + e.toString());
    } catch (IOException e) {
        Log.e("login activity", "Can not read file: " + e.toString());
    }
    return ret;
}
```

- **Q1:** To solve the thread-safety issue of the code, we can modify the API "StringBuilder" to:
- **Q2:** If you didn’t know the problem before, do you think the results recommend by the tool help you to realize the problem? (answer yes/no)

The questions are derived from the following SO posts and comments:
- https://stackoverflow.com/questions/355089/difference-between-stringbuilder-and-stringbuffer


