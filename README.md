# Phonebook
This program reads in names and the corresponding phone number from a file and stores them into parallel arrays. It then returns the phone number when the name is entered, and vice versa.

import java.util.Scanner;
import java.io.*;

public class Phonebook {
    public static void main(String [] args) throws Exception {
        final String FILENAME = "phonebook.text";
        final int CAPACITY = 100;
        String []
                last = new String[CAPACITY],
                first = new String[CAPACITY],
                numbers = new String[CAPACITY];
        int size = read(FILENAME, first, last, numbers, CAPACITY);
        Scanner sc = new Scanner(System.in);
        String firstName, lastName, number;
        char choice;
        int lookupCount = 0, reverseLookupCount = 0;
        System.out.print("lookup, reverse-lookup, quit (l/r/q)?");
        choice = sc.next().charAt(0);
        while(choice != 'q') {
            if(choice == 'l') {
                System.out.print("last name? ");
                lastName = sc.next();
                System.out.print("first name? ");
                firstName = sc.next();
                number = lookup(last, first, numbers, lastName, firstName, size);
                if(!number.equals(" "))
                    System.out.println(firstName + " " + lastName + "'s phone number is " + number);
                else
                    System.out.println("-- Name not found");
                lookupCount++;
            }
            else if(choice == 'r') {
                System.out.print("phone number (nnn-nnn-nnnn)?");
                number = sc.next();
                firstName = reverseLookup(first, numbers, number, size);
                lastName = reverseLookup(last, numbers, number, size);
                if((!firstName.equals(" ")) || (!lastName.equals(" ")))
                    System.out.println(number + " belongs to " + lastName + ", " + firstName);
                else
                    System.out.println("-- Phone number not found");
                reverseLookupCount++;
            }
            System.out.print("lookup, reverse-lookup, quit (l/r/q)?");
            choice = sc.next().charAt(0);
        }
        System.out.println(lookupCount + " lookups performed\n" + reverseLookupCount + " reverse lookups performed");
    }
    public static String lookup(String [] last, String [] first, String [] numbers, String lastName, String firstName, int size) {
        for(int i = 0; i < size; i++)
            if((first[i].equals(firstName)) && (last[i].equals(lastName)))
                return numbers[i];
            return " ";
    }
    public static String reverseLookup(String [] array, String [] numbers, String number, int size) {
        for(int i = 0; i < size; i++)
            if(numbers[i].equals(number))
                return array[i];
            return " ";
    }
    static int read(String filename, String [] first, String [] last, String [] numbers, int capacity) throws IOException {
        Scanner scanner = new Scanner(new File(filename));
        int size = 0;
        while (scanner.hasNext()) {
            if (size == capacity) {
                System.out.println("Phonebook capacity exceeded - increase size of underlying array");
                System.exit(1);
            }
            last[size] = scanner.next();
            first[size] = scanner.next();
            numbers[size] = scanner.next();
            size++;
        }
        return size;
    }
}

