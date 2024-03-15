# Project2

import java.util.*;

public class Main {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        List<Integer> numbers = new ArrayList<>();

        while (scanner.hasNext()) {
            String line = scanner.nextLine();
            String[] tokens = line.split("\\s+");

            for (String token : tokens) {
                try {
                    int num = Integer.parseInt(token);
                    numbers.add(num);
                } catch (NumberFormatException e) {
                    
                }
            }
        }

      
        List<String> binaryNumbers = new ArrayList<>();
        for (int num : numbers) {
            binaryNumbers.add(decimalToBinary(num));
        }

       
        mergeSort(binaryNumbers);

        double median;
        if (binaryNumbers.size() % 2 == 0) {
            median = (binaryToDecimal(binaryNumbers.get(binaryNumbers.size() / 2)) +
                    binaryToDecimal(binaryNumbers.get(binaryNumbers.size() / 2 - 1))) / 2.0;
        } else {
            median = binaryToDecimal(binaryNumbers.get(binaryNumbers.size() / 2));
        }

        double average = 0;
        for (String binaryNum : binaryNumbers) {
            average += binaryToDecimal(binaryNum);
        }
        average /= binaryNumbers.size();

        System.out.println("Медіана: " + median);
        System.out.println("Середнє значення: " + average);
    }

    private static String decimalToBinary(int num) {
        return Integer.toBinaryString(num & 0xFFFF); 
    }

    private static int binaryToDecimal(String binary) {
        return Integer.parseInt(binary, 2);
    }

    private static void mergeSort(List<String> arr) {
        if (arr.size() < 2) {
            return;
        }

        int mid = arr.size() / 2;
        List<String> left = new ArrayList<>(arr.subList(0, mid));
        List<String> right = new ArrayList<>(arr.subList(mid, arr.size()));

        mergeSort(left);
        mergeSort(right);
        merge(left, right, arr);
    }

    private static void merge(List<String> left, List<String> right, List<String> arr) {
        int leftIndex = 0, rightIndex = 0, arrIndex = 0;

        while (leftIndex < left.size() && rightIndex < right.size()) {
            if (left.get(leftIndex).compareTo(right.get(rightIndex)) <= 0) {
                arr.set(arrIndex++, left.get(leftIndex++));
            } else {
                arr.set(arrIndex++, right.get(rightIndex++));
            }
        }

        while (leftIndex < left.size()) {
            arr.set(arrIndex++, left.get(leftIndex++));
        }

        while (rightIndex < right.size()) {
            arr.set(arrIndex++, right.get(rightIndex++));
        }
    }
}
