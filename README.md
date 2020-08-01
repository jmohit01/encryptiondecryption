# encryptiondecryption

package encryptdecrypt;

import java.io.File;
import java.util.Scanner;
import java.io.FileWriter;
import java.io.PrintWriter;
import java.io.IOException;
import java.io.FileNotFoundException;

public class EncDec {
    static String filePathOne = null;
    static String filePathTwo = null;

    public static void main(String[] args) throws IOException {

        int key = 0;
        String message = null;
        String mode = null;
        String out = null;
        String data = null;
        String alg = null;

        for (int i = 0; i < args.length; i++) {

            if (args[i].equals("-mode")) {

                if (args[i + 1].equals("dec")) {

                    mode = "dec";

                } else if (args[i + 1].equals("enc")) {

                    mode = "enc";
                }

            } else if (args[i].equals("-in")) {

                filePathOne = args[i + 1];

                File file = new File(filePathOne);

                if (file.exists() != true) {

                    file.createNewFile();
                }

                if (file.length() == 0) {
                    try (PrintWriter writer = new PrintWriter(file)) {

                        writer.write("Welcome to hyperskill!");
                
                    } catch (FileNotFoundException e) {
                        System.out.println("File not availble: " + filePathOne);
                    }
                }
                try (Scanner scanner = new Scanner(file)) {
                    while (scanner.hasNext()) {
                        message = scanner.nextLine();
                    }
                } catch (FileNotFoundException e) {
                    System.out.println("File not availble: " + filePathOne);
                }

            } else if (args[i].equals("-out")) {

                out = "out";
                filePathTwo = args[i + 1];

            } else if (args[i].equals("-key")) {

                key = Integer.parseInt(args[i + 1]);

            } else if (args[i].equals("-data")) {

                message = args[i + 1];
            
            } else if (args[i].equals("-alg")) {

                alg = args[i + 1];
            }
        }

        if (mode == "dec") {

            if (out == "out") {
                EncDec.decryptionToFile(message, key, alg, filePathTwo);
            } else {
                EncDec.decryption(message, key, alg);
            }

        } else {

            if (out == "out") {
                EncDec.encryptionToFile(message, key, alg, filePathTwo);
            } else {
                EncDec.encryption(message, key, alg);
            }
        }
    }

    public static void encryptionToFile(String message, int key, String alg, String filePathTwo) throws IOException {

        char[] charArray = new char[message.length()];
        char[] cArray = new char[charArray.length];
        File file = new File(filePathTwo);

        if (!file.exists()) {
            file.createNewFile();
        }

        if (alg.equals("unicode")) {
            try (PrintWriter printWriter = new PrintWriter(file)) {

                for (int i = 0; i < message.length(); i++) {

                    cArray[i] = (char) (message.charAt(i) + key);
                }

                String msg = String.valueOf(cArray);
                printWriter.write(msg);

            } catch (FileNotFoundException e) {
                System.out.println("File not found: " + filePathTwo);
            } 
        
        } else if (alg.equals("shift")) {

            for (int i = 0; i < charArray.length; i++) {
                charArray[i] = (char) (message.charAt(i));
            }

            int result = 0;

            for (int i = 0; i < charArray.length; i++) {
                    
                if (Character.isUpperCase(charArray[i])) {

                    result = (charArray[i] + key - 65) % 26 + 65;
                    cArray[i] = (char) (result);
                
                } else if (Character.isLowerCase(charArray[i])) {
                    result = (charArray[i] + key - 97) % 26 + 97;
                    cArray[i] = (char) (result);
                        
                } else {
                    
                    cArray[i] = charArray[i];
                }
            }
            
            FileWriter writer = new FileWriter(file);
            writer.write(String.valueOf(cArray));
            writer.flush();
            writer.close();
        }  
    }

    public static void decryptionToFile(String message, int key, String alg, String filePathTwo) throws IOException {
        char[] charArray = message.toCharArray();
        char[] cArray = new char[charArray.length];

        File file = new File(filePathTwo);

        if (file.exists() != true) {
            file.createNewFile();
        }

        if (alg.equals("unicode")) {
            
            try (FileWriter writer = new FileWriter(file)) {

                for (int i = 0; i < charArray.length; i++) {

                    cArray[i] = (char) (charArray[i] - key);
                }

                writer.write(String.valueOf(cArray));

            } catch (FileNotFoundException e) {
                System.out.println("File not found: " + filePathTwo);
            }
        
        } else if (alg.equals("shift")) {
            int result = 0;

            for (int i = 0; i < charArray.length; i++) {
                result = 0;
                
                if (Character.isUpperCase(charArray[i])) {

                    result = (charArray[i] - key - 65) % 26;

                    if (result < 0) {

                        result = 26 + (result);
                    }

                    cArray[i] = (char) (result + 65);
                
                } else if (Character.isLowerCase(charArray[i])) {

                    result = (charArray[i] - key - 97) % 26;

                    if (result < 0) {

                        result = 26 + (result);
                    }

                    cArray[i] = (char) (result + 97);
                
                } else {

                    cArray[i] = charArray[i];
                }
            }
            
            FileWriter writer = new FileWriter(file);
            writer.write(String.valueOf(cArray));
            writer.flush();
            writer.close();
        }
    }

    public static void encryption(String message, int key, String alg) {

        char[] charArray = message.toCharArray();
        char[] cArray = new char[charArray.length];

        if (alg.equals("unicode")) {
            for (int i = 0; i < message.length(); i++) {

                cArray[i] = (char) (message.charAt(i) + key);
            }

            System.out.println(String.valueOf(cArray));

        } else if (alg.equals("shift")) {

            int result = 0;

            for (int i = 0; i < charArray.length; i++) {
                result = 0;
                
                if (Character.isUpperCase(charArray[i])) {

                    result = (charArray[i] + key - 65) % 26 + 65;
                    cArray[i] = (char) (result);
                
                } else if (Character.isLowerCase(charArray[i])) {

                    result = (charArray[i] + key - 97) % 26 + 97;
                    cArray[i] = (char) (result);
                
                } else {

                    cArray[i] = charArray[i];
                }
            }
            System.out.println(String.valueOf(cArray));
        }
    }

    public static void decryption(String message, int key, String alg) {

        char[] charArray = message.toCharArray();
        char[] cArray = new char[charArray.length];

        if (alg.equals("unicode")) {
            for (int i = 0; i < charArray.length; i++) {

                cArray[i] = (char) (charArray[i] - key);
            }

            String msgOne = String.valueOf(cArray);
            System.out.println(msgOne);
        
        } else if (alg.equals("shift")) {

            int result = 0;

            for (int i = 0; i < charArray.length; i++) {
                result = 0;
                
                if (Character.isUpperCase(charArray[i])) {

                    result = (charArray[i] - key - 65) % 26;

                    if (result < 0) {

                        result = 26 + (result);
                    }

                    cArray[i] = (char) (result + 65);
                
                } else if (Character.isLowerCase(charArray[i])) {

                    result = (charArray[i] - key - 97) % 26;

                    if (result < 0) {

                        result = 26 + (result);
                    }

                    cArray[i] = (char) (result + 97);
                
                } else {

                    cArray[i] = charArray[i];
                }
            }
            System.out.println(String.valueOf(cArray));
        }
    }
}
