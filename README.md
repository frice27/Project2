import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {
    public static void main(String[] args) {
        System.out.println("Enter numbers separated by spaces: ");
        String input = getInput();

        String[] words = input.split(" ");

        while (words.length == 0) {
            System.out.println("No numbers entered. Please enter numbers separated by spaces: ");
            input = getInput();
            words = input.split(" ");
        }

        System.out.println("Array created with the following numbers:");
        for (String word : words) {
            System.out.println(word);
        }

        int[] integers = convertWordsToIntegers(words);

        System.out.println("Integers converted from words:");
        for (int num : integers) {
            System.out.println(num);
        }

        System.out.println("Binary representation of numbers in two's complement:");
        String[] binaryArray = toTwosComplementArray(integers);
        for (int num : integers) {
            String twosComplement = toTwosComplement(num);
            System.out.println(num + " : " + twosComplement);
        }

        System.out.println("Sorted array: ");
        String[] sortedBinaries = performBubbleSort(binaryArray);
        for (String sortedWord : sortedBinaries) {
            System.out.println(sortedWord);
        }

        String median = calculateMedian(binaryArray);
        System.out.println("Median: " + binaryToDecimal(median));

        String mean = calculateMean(binaryArray);
        System.out.println("Mean: " + mean);
    }

    public static String calculateMedian(String[] binaryArray) {
        int length = binaryArray.length;
        String[] sortedBinaries = performBubbleSort(binaryArray);

        if (length % 2 == 0) {
            String firstMiddle = sortedBinaries[length / 2 - 1];
            String secondMiddle = sortedBinaries[length / 2];
            return calculateMean(new String[]{firstMiddle, secondMiddle});
        } else {
            return sortedBinaries[length / 2];
        }
    }

    public static String calculateMean(String[] binaryArray) {
        int sum = 0;
        for (String binary : binaryArray) {
            sum = addBinary(sum, binaryToInteger(binary));
        }

        double mean = (double) sum / binaryArray.length;

        return Double.toString(mean);
    }

    public static int binaryToInteger(String binary) {
        int result = 0;
        for (int i = 0; i < binary.length(); i++) {
            if (binary.charAt(i) == '1') {
                result += Math.pow(2, binary.length() - 1 - i);
            }
        }
        return result;
    }

    public static int addBinary(int a, int b) {
        while (b != 0) {
            int carry = a & b;
            a = a ^ b;
            b = carry << 1;
        }
        return a;
    }

    public static int binaryToDecimal(String binary) {
        int decimal = 0;
        for (int i = 0; i < binary.length(); i++) {
            if (binary.charAt(i) == '1') {
                decimal += Math.pow(2, binary.length() - 1 - i);
            }
        }
        return decimal;
    }

    public static String[] performBubbleSort(String[] arr) {
        String[] sortedArray = arr.clone();

        int n = sortedArray.length;
        boolean swapped;
        do {
            swapped = false;
            for (int i = 1; i < n; i++) {
                if (compareBinaryStrings(sortedArray[i - 1], sortedArray[i]) > 0) {
                    String temp = sortedArray[i - 1];
                    sortedArray[i - 1] = sortedArray[i];
                    sortedArray[i] = temp;
                    swapped = true;
                }
            }
            n--;
        } while (swapped);

        return sortedArray;
    }

    public static String[] toTwosComplementArray(int[] integers) {
        String[] twosComplementArray = new String[integers.length];
        for (int i = 0; i < integers.length; i++) {
            twosComplementArray[i] = toTwosComplement(integers[i]);
        }
        return twosComplementArray;
    }

    public static int[] convertWordsToIntegers(String[] words) {
        int[] integers = new int[words.length];

        for (int i = 0; i < words.length; i++) {
            try {
                integers[i] = Integer.parseInt(words[i]);
            } catch (NumberFormatException e) {
                integers[i] = 0;
            }
        }

        return integers;
    }

    public static String toTwosComplement(int num) {
        int maxValue = (int) Math.pow(2, 15) - 1;
        int minValue = -(int) Math.pow(2, 15);

        if (num > maxValue)
            num = maxValue;
        else if (num < minValue)
            num = maxValue;

        StringBuilder binaryRepresentation = new StringBuilder();

        if (num < 0) {
            num = Math.abs(num);
            while (num > 0) {
                binaryRepresentation.insert(0, num % 2);
                num /= 2;
            }

            while (binaryRepresentation.length() < 16) {
                binaryRepresentation.insert(0, "0");
            }

            for (int i = 0; i < binaryRepresentation.length(); i++) {
                if (binaryRepresentation.charAt(i) == '0') {
                    binaryRepresentation.setCharAt(i, '1');
                } else {
                    binaryRepresentation.setCharAt(i, '0');
                }
            }

            int carry = 1;
            for (int i = binaryRepresentation.length() - 1; i >= 0; i--) {
                int digit = binaryRepresentation.charAt(i) - '0' + carry;
                binaryRepresentation.setCharAt(i, (char) ('0' + digit % 2));
                carry = digit / 2;
            }
        } else {
            while (num > 0) {
                binaryRepresentation.insert(0, num % 2);
                num /= 2;
            }

            while (binaryRepresentation.length() < 16) {
                binaryRepresentation.insert(0, "0");
            }
        }
        return binaryRepresentation.toString();
    }

    public static int compareBinaryStrings(String str1, String str2) {
        int len1 = str1.length();
        int len2 = str2.length();
        if (len1 != len2) {
            return len1 - len2;
        }
        return str1.compareTo(str2);
    }

    public static String getInput() {
        String input = "";

        try {
            InputStreamReader isr = new InputStreamReader(System.in);
            BufferedReader br = new BufferedReader(isr);
            input = br.readLine();
        } catch (IOException e) {
            System.out.println("Error reading input. Please try again.");
        }

        return input;
    }
}

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public final class DataInput {
    public DataInput() {
    }

    public static Long getLong() {
        String s = getString();

        try {
            return Long.valueOf(s);
        } catch (NumberFormatException var2) {
            System.out.println("Invalid input. Please restart and enter a valid Long value.");
            return null;
        }
    }

    public static char getChar() {
        String s = getString();
        if (s.length() == 0) {
            System.out.println("Empty input. Please restart and enter a character.");
            return '\u0000'; // Returns null character if input is empty
        } else {
            return s.charAt(0);
        }
    }

    public static Double getDouble() {
        int program = 1;
        while (program == 1) {
            String s = getString();
            try {
                Double result = Double.valueOf(s);
                program = 0;
                return result;
            } catch (NumberFormatException var2) {
                System.out.println("Invalid input. Please enter a valid double value.");
                program = 1;
            }
        }
        return null; // Returns null if input is not a valid double
    }

    public static Integer getInt() {
        int program = 1;

        while (program == 1) {
            String s = getString();

            try {
                Integer result = Integer.valueOf(s);
                program = 0;
                return result;
            } catch (NumberFormatException var2) {
                System.out.println("Invalid input. Please enter a valid integer value.");
                program = 1;
            }
        }

        return null; // Returns null if input is not a valid integer
    }

    public static String getString() {
        String s = "";

        try {
            InputStreamReader isr = new InputStreamReader(System.in);
            BufferedReader br = new BufferedReader(isr);
            s = br.readLine(); // Reads input from the console
        } catch (IOException var3) {
            System.out.println("Error reading input. Please try again.");
        }

        return s; // Returns the input string
    }

}

