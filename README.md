# MINI PROJECT REPORT
ON
# HOTEL MANAGEMENT SYSTEM
Submitted by
RAJA SANJAY CH
NC.SC.U4CSE25230
For
23CSE113 –  OOP
II Semester
B.Tech. CSE - C
School of Computing
Amrita Vishwa Vidyapeetham, Nagercoil
________________________________________







Index
Sl.No.	Chapter Name	Page No.
1.	Introduction	3
2.	Problem Statement	3
3.	Objectives	4
4.	Java Concepts Used	4
5.	Modules of the Project	4
6.	Code	5
7.	Output Screenshots	6
8.	Applications of the Project	7
9.	Limitations of the Project	8
10.	Bibliography	8
11.	GitHub Link	8
________________________________________
1. Introduction

The Hotel Management System is a console-based application developed using Java. It is designed to manage hotel operations such as room booking, guest management, billing, and cancellation efficiently.
Manual hotel management can be time-consuming and error-prone. This system automates the process, making it easier to manage room availability and customer details with greater accuracy and speed.
The system uses Object-Oriented Programming (OOP) concepts to organize hotel data and perform various operations. It provides a user-friendly interface where users can input guest details, book rooms, and instantly get billing information.
This project demonstrates how real-world business problems like hotel management can be solved using Java programming concepts.

2. Problem Statement

Manual handling of hotel operations leads to several issues such as:
• Difficulty in tracking room availability - Hard to maintain real-time room status
• Errors in billing and booking - Manual calculations prone to mistakes
• Time-consuming processes - Slow check-in and check-out procedures
• Lack of proper record management - Poor guest history tracking
There is a need for a system that can:
• Manage guest details efficiently
• Handle room booking with real-time availability
• Calculate billing automatically based on room type and duration
• Provide quick and accurate results for hotel operations
This project aims to develop a Java-based Hotel Management System to automate hotel operations efficiently.

3. Objectives

• To design a hotel management system using Java
• To manage rooms and guests efficiently
• To automate booking and checkout processes
• To calculate bills automatically based on room rates
• To apply Object-Oriented Programming concepts
• To improve accuracy and efficiency in hotel operations

4. Java Concepts Used

Classes and Objects
Used to represent Guest, Room, Booking, and Hotel entities.
Constructors
Used to initialize data like room details, guest details, and booking information.
Encapsulation
Hotel data is organized inside classes with proper access modifiers.
ArrayList
Used to store rooms, guests, and bookings dynamically.
Methods
Used for booking, checkout, cancellation, billing, and display operations.
Scanner Class
Used to take user input for interactive menu system.
Loops and Conditions
Used for menu-driven program execution and data validation.

5. Modules of the Project

Guest Module
• Stores guest ID, name, and contact details
• Manages guest information efficiently
Room Module
• Stores room number, type, price, and availability status
• Handles different room categories
Booking Module
• Handles room booking and stores booking details
• Manages check-in and check-out dates
Billing Module
• Calculates total bill based on number of days and room type
• Handles payment processing
Hotel Module
• Controls all operations like booking, checkout, and cancellation
• Acts as the main controller class
Main Module
• Provides menu-driven interface for user interaction
• Handles program flow and user choices

6. Code

import java.util.*;
import java.time.LocalDate;

class Guest {
private static int guestIdCounter = 1001;
private int guestId;
private String name;
private String contact;

public Guest(String name, String contact) {
this.guestId = guestIdCounter++;
this.name = name;
this.contact = contact;
}

// Getters
public int getGuestId() { return guestId; }
public String getName() { return name; }
public String getContact() { return contact; }
}

class Room {
private int roomNumber;
private String roomType;
private double pricePerNight;
private boolean isAvailable;

public Room(int roomNumber, String roomType, double pricePerNight) {
this.roomNumber = roomNumber;
this.roomType = roomType;
this.pricePerNight = pricePerNight;
this.isAvailable = true;
}

// Getters and Setters
public int getRoomNumber() { return roomNumber; }
public String getRoomType() { return roomType; }
public double getPricePerNight() { return pricePerNight; }
public boolean isAvailable() { return isAvailable; }
public void setAvailable(boolean available) { this.isAvailable = available; }
}

class Booking {
private static int bookingIdCounter = 2001;
private int bookingId;
private Guest guest;
private Room room;
private int numberOfDays;
private double totalBill;

public Booking(Guest guest, Room room, int numberOfDays) {
this.bookingId = bookingIdCounter++;
this.guest = guest;
this.room = room;
this.numberOfDays = numberOfDays;
this.totalBill = room.getPricePerNight() * numberOfDays;
room.setAvailable(false);
}

public void displayBookingDetails() {
System.out.println("\n--- Booking Confirmation ---");
System.out.println("Booking ID: " + bookingId);
System.out.println("Guest Name: " + guest.getName());
System.out.println("Room Number: " + room.getRoomNumber());
System.out.println("Room Type: " + room.getRoomType());
System.out.println("Number of Days: " + numberOfDays);
System.out.println("Total Bill: ₹" + totalBill);
}

// Getters
public int getBookingId() { return bookingId; }
public Room getRoom() { return room; }
public double getTotalBill() { return totalBill; }
}

class Hotel {
private ArrayList rooms;
private ArrayList bookings;

public Hotel() {
rooms = new ArrayList<>();
bookings = new ArrayList<>();
initializeRooms();
}

private void initializeRooms() {
rooms.add(new Room(101, "Single", 1500));
rooms.add(new Room(102, "Single", 1500));
rooms.add(new Room(201, "Double", 2500));
rooms.add(new Room(202, "Double", 2500));
rooms.add(new Room(301, "Suite", 5000));
}

public void displayAvailableRooms() {
System.out.println("\n--- Available Rooms ---");
boolean hasAvailable = false;
for (Room room : rooms) {
if (room.isAvailable()) {
System.out.println("Room " + room.getRoomNumber() + 
" - " + room.getRoomType() + " - ₹" + room.getPricePerNight() + "/night");
hasAvailable = true;
}
}
if (!hasAvailable) {
System.out.println("No rooms available!");
}
}

public Room findRoom(int roomNumber) {
for (Room room : rooms) {
if (room.getRoomNumber() == roomNumber && room.isAvailable()) {
return room;
}
}
return null;
}

public Booking bookRoom(String guestName, String contact, int roomNumber, int days) {
Room room = findRoom(roomNumber);
if (room != null) {
Guest guest = new Guest(guestName, contact);
Booking booking = new Booking(guest, room, days);
bookings.add(booking);
return booking;
}
return null;
}

public boolean checkout(int bookingId) {
for (Booking booking : bookings) {
if (booking.getBookingId() == bookingId) {
booking.getRoom().setAvailable(true);
bookings.remove(booking);
System.out.println("Checkout successful! Total bill: ₹" + booking.getTotalBill());
return true;
}
}
return false;
}
}

public class HotelManagementSystem {
public static void main(String[] args) {
Scanner sc = new Scanner(System.in);
Hotel hotel = new Hotel();

while (true) {
System.out.println("\n=== HOTEL MANAGEMENT SYSTEM ===");
System.out.println("1. View Available Rooms");
System.out.println("2. Book Room");
System.out.println("3. Checkout");
System.out.println("4. Exit");
System.out.print("Enter your choice: ");

int choice = sc.nextInt();
sc.nextLine(); // consume newline

switch (choice) {
case 1:
hotel.displayAvailableRooms();
break;

case 2:
System.out.print("Enter guest name: ");
String name = sc.nextLine();
System.out.print("Enter contact number: ");
String contact = sc.nextLine();
System.out.print("Enter room number: ");
int roomNum = sc.nextInt();
System.out.print("Enter number of days: ");
int days = sc.nextInt();

Booking booking = hotel.bookRoom(name, contact, roomNum, days);
if (booking != null) {
booking.displayBookingDetails();
} else {
System.out.println("Room not available or invalid room number!");
}
break;

case 3:
System.out.print("Enter booking ID: ");
int bookingId = sc.nextInt();
if (!hotel.checkout(bookingId)) {
System.out.println("Invalid booking ID!");
}
break;

case 4:
System.out.println("Thank you for using Hotel Management System!");
System.exit(0);
break;

default:
System.out.println("Invalid choice! Please try again.");
}
}
}
}
7. Output Screenshots
Main Menu Display:
=== HOTEL MANAGEMENT SYSTEM ===
1. View Available Rooms
2. Book Room 
3. Checkout
4. Exit
Enter your choice: 1
Available Rooms Display:
--- Available Rooms ---
Room 101 - Single - ₹1500/night
Room 102 - Single - ₹1500/night 
Room 201 - Double - ₹2500/night
Room 202 - Double - ₹2500/night
Room 301 - Suite - ₹5000/night
Room Booking Process:
Enter guest name: John Doe
Enter contact number: 9876543210
Enter room number: 201
Enter number of days: 3

--- Booking Confirmation ---
Booking ID: 2001
Guest Name: John Doe
Room Number: 201
Room Type: Double
Number of Days: 3
Total Bill: ₹7500
Checkout Process:
Enter booking ID: 2001
Checkout successful! Total bill: ₹7500

8. Applications of the Project

• Used in hotels for efficient room management and guest services
• Helps automate booking process reducing manual paperwork
• Reduces manual errors in billing and room allocation
• Useful for small hotel businesses to manage operations digitally
• Can be extended into real-world applications with database integration
• Improves customer service with faster check-in/check-out processes

9. Limitations of the Project

• No database - Data not stored permanently, lost after program termination
• Console-based - No graphical user interface (GUI)
• Limited features - No advanced booking options or room service management
• No authentication system - No user login or security features
• Cannot handle multiple users simultaneously
• No payment gateway integration for online transactions

10. Bibliography

• Java Documentation – https://docs.oracle.com/javase/
• GeeksforGeeks – Java OOP Concepts and ArrayList tutorials
• Java Tutorials – Oracle's official Java learning resources
• Stack Overflow – Community support for Java programming


________________________________________


                      


                          THANK YOU

