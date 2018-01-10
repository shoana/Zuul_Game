# Zuul_Game
A user friendly game that allowed players to acquire items and visit different locations
import java.util.*;
/**
 * This class is part of the "World of Zuul" application. 
 * "World of Zuul" is a very simple, text based adventure game.  
 *
 * This class holds information about a command that was issued by the user.
 * A command currently consists of two strings: a command word and a second
 * word (for example, if the command was "take map", then the two strings
 * obviously are "take" and "map").
 * 
 * The way this is used is: Commands are already checked for being valid
 * command words. If the user entered an invalid command (a word that is not
 * known) then the command word is <null>.
 *
 * If the command had only one word, then the second word is <null>.
 * 
 * @author  Michael Kolling and David J. Barnes
 * @version 2006.03.30
 * 
 * @author Lynn Marshall
 * @version October 21, 2012
 * 
 * @author Shoana Sharma
 * @version October 21, 2017
 */

public class Command
{
    private String commandWord;
    private String secondWord;

    /**
     * Create a command object. First and second word must be supplied, but
     * either one (or both) can be null.
     * 
     * @param firstWord The first word of the command. Null if the command
     *                  was not recognised
     * @param secondWord The second word of the command
     */
    public Command(String firstWord, String secondWord)
    {
        commandWord = firstWord;
        this.secondWord = secondWord;
    }

    /**
     * Return the command word (the first word) of this command. If the
     * command was not understood, the result is null.
     * 
     * @return The command word, or null if not understood
     */
    public String getCommandWord()
    {
        return commandWord;
    }

    /**
     * Returns the second word of the command.  Returns null if there was no
     * second word.
     * 
     * @return The second word of this command, or null if only one word
     */
    public String getSecondWord()
    {
        return secondWord;
    }

    /**
     * Returns true if the command is unknown.
     * 
     * @return true if this command was not understood, false otherwise
     */
    public boolean isUnknown()
    {
        return (commandWord == null);
    }

    /**
     * Returns true if the command has a second word.
     * 
     * @return true if the command has a second word, false otherwise
     */
    public boolean hasSecondWord()
    {
        return (secondWord != null);
    }
}

/**
 * This class is part of the "World of Zuul" application. 
 * "World of Zuul" is a very simple, text based adventure game.
 * 
 * This class holds an enumeration of all command words known to the game.
 * It is used to recognise commands as they are typed in.
 *
 * @author  Michael Kolling and David J. Barnes
 * @version 2006.03.30
 * 
 * @author Lynn Marshall
 * @version A3 Solution 
 * 
 * @author Shoana Sharma
 * @version Novermber.12.2017
 * 
 * Using Professor Marshall's solution from assignment 3
 */

public class CommandWords
{
    // a constant array that holds all valid command words
    private static final String[] validCommands = {
        "go", "quit", "help", "look", "eat", "back", "stackBack","take", "drop","charge", "fire"
      

    };

    /**
     * Constructor - initialise the command words.
     */
    public CommandWords()
    {
        // nothing to do at the moment...
    }

    /**
     * Check whether a given String is a valid command word. 
     * 
     * @param aString The String to check
     * @return true if it is valid, false otherwise
     */
    public boolean isCommand(String aString)
    {
        for(int i = 0; i < validCommands.length; i++) {
            if(validCommands[i].equals(aString))
                return true;
        }
        // if we get here, the string was not found in the commands
        return false;
    }

    /**
     * Returns a String containing all valid commands.
     * 
     * @return a String of the valid commands
     */
    public String getCommandList() 
    {
        // let's use a StringBuilder (not required)
        StringBuilder s = new StringBuilder();
        for(String command: validCommands) {
            s.append(command + "  ");
        }
        String str = s.toString(); 
        return str.trim(); // removes spaces from beginning/end
    }
}

import java.util.*;

/**
 *  This class is the main class of the "World of Zuul" application. 
 *  "World of Zuul" is a very simple, text based adventure game.  Users 
 *  can walk around some scenery. That's all. It should really be extended 
 *  to make it more interesting!
 * 
 *  To play this game, create an instance of this class and call the "play"
 *  method.
 * 
 *  This main class creates and initialises all the others: it creates all
 *  rooms, creates the parser and starts the game.  It also evaluates and
 *  executes the commands that the parser returns.
 * 
 * @author  Michael Kolling and David J. Barnes 
 * @version 2006.03.30
 * 
 * @author Lynn Marshall
 * @version A3 Solution
 * 
 * @author Shoana Sharma
 * @version Novermber.12.2017
 * 
 * Using Professor Marshall's solution from assignment 3
 */

public class Game 
{
    private Parser parser;
    private Room currentRoom;
    private Room previousRoom;
    private Stack<Room> previousRoomStack;

    private Item currItem;
    private int energy = 5;
    private Beamer currBeamer;
    private ArrayList<Room> roomTransport;
    private TransporterRoom transport;

    /**
     * Create the game and initialise its internal map, as well
     * as the previous room (none) and previous room stack (empty).
     */
    public Game() 
    {

        parser = new Parser();
        previousRoom = null;
        previousRoomStack = new Stack<Room>();

        currentRoom =this.currentRoom;
        currItem = this.currItem;
        energy = this.energy;
        currBeamer = this.currBeamer;
        roomTransport = new ArrayList<Room>();
        transport= this.transport;
        createRooms();
    }

    /**
     * Create all the rooms and link their exits together. New additions include transporter and beamer.
     */

    private void createRooms()
    {

        Room outside, theatre, pub, lab, office;
        Item chair, bar, computer, computer2, tree, candy, beamer;

        // create some items
        chair = new Item("a wooden chair",5, "chair");
        bar = new Item("a long bar with stools",95.67, "bar");
        computer = new Item("a PC",10, "computer");
        computer2 = new Item("a Mac",5, "computer2");
        tree = new Item("a fir tree",500.5, "tree");
        candy = new Item("sweet yummy thing", 1.0, "candy");
        beamer = new Beamer("a weapon", 1.0, "beamer");

        // create the rooms
        outside = new Room("outside the main entrance of the university");
        theatre = new Room("in a lecture theatre");
        pub = new Room("in the campus pub");
        lab = new Room("in a computing lab");
        office = new Room("in the computing admin office");
        transport = new TransporterRoom("in a mysterious Transporter room",roomTransport); //new room

        //for TranporterRoom

        roomTransport.add(outside);
        roomTransport.add(theatre);
        roomTransport.add(pub);
        roomTransport.add(lab);
        roomTransport.add(office);
        roomTransport.add(transport);

        // put items in the rooms
        outside.addItem(tree);
        outside.addItem(tree);
        theatre.addItem(chair);
        pub.addItem(bar);
        lab.addItem(chair);
        lab.addItem(computer);
        lab.addItem(chair);
        lab.addItem(computer2);
        office.addItem(chair);
        office.addItem(computer);
        theatre.addItem(candy);
        lab.addItem(candy);
        office.addItem(candy);
        pub.addItem(candy);
        lab.addItem(beamer);
        theatre.addItem(beamer);

        // initialise room exits
        outside.setExit("east", theatre);

        outside.setExit("south", lab);
        outside.setExit("west", pub);
        outside.setExit("north", transport); //to find transporter
        transport.setExit("east",outside);

        theatre.setExit("west", outside);

        pub.setExit("east", outside);

        lab.setExit("north", outside);
        lab.setExit("east", office);

        office.setExit("west", lab);

        currentRoom = outside;  // start game outside
    }

    /**
     *  Main play routine.  Loops until end of play.
     */
    public void play() 
    {            
        printWelcome();

        // Enter the main command loop.  Here we repeatedly read commands and
        // execute them until the game is over.

        boolean finished = false;
        while (! finished) {
            Command command = parser.getCommand();
            finished = processCommand(command);
        }
        System.out.println("Thank you for playing.  Good bye.");
    }

    /**
     * Print out the opening message for the player.
     */
    private void printWelcome()
    {
        System.out.println();
        System.out.println("Welcome to the World of Zuul!");
        System.out.println("World of Zuul is a new, incredibly boring adventure game.");
        System.out.println("Type 'help' if you need help.");
        System.out.println();
        System.out.println(currentRoom.getLongDescription());
    }

    /**
     * Given a command, process (that is: execute) the command.
     * 
     * @param command The command to be processed
     * @return true If the command ends the game, false otherwise
     */
    private boolean processCommand(Command command) 
    {
        boolean wantToQuit = false;

        if(command.isUnknown()) {
            System.out.println("I don't know what you mean...");
            return false;
        }

        String commandWord = command.getCommandWord();
        if (commandWord.equals("help")) {
            printHelp();
        }
        else if (commandWord.equals("go")) {
            goRoom(command);
        }
        else if (commandWord.equals("quit")) {
            wantToQuit = quit(command);
        }
        else if (commandWord.equals("look")) {
            look(command);
        }
        else if (commandWord.equals("eat")) {
            eat(command);
        }
        else if (commandWord.equals("back")) {
            back(command);
        }
        else if (commandWord.equals("stackBack")) {
            stackBack(command);
        }
        else if(commandWord.equals("take")){
            take(command);
        }
        else if(commandWord.equals("drop")){
            drop(command);
        }
        else if(commandWord.equals("fire")){
            fire(command);
        }
        else if(commandWord.equals("charge")){
            charge(command);
        }
        // else command not recognised.
        return wantToQuit;
    }

    /**
     * Print out some help information.
     * Here we print a cryptic message and a list of the 
     * command words.
     */
    private void printHelp() 
    {
        System.out.println("You are lost. You are alone. You wander");
        System.out.println("around at the university.");
        System.out.println();
        System.out.println("Your command words are:");
        System.out.println(parser.getCommands());
    }

    /** 
     * Try to go to one direction. If there is an exit, enter the new
     * room, otherwise print an error message.
     * If we go to a new room, update previous room and previous room stack.
     * 
     * @param command The command to be processed
     */
    private void goRoom(Command command) 
    {
        if(!command.hasSecondWord()) 
        {
            // if there is no second word, we don't know where to go...
            System.out.println("Go where?");
            return;
        }

        String direction = command.getSecondWord();

        // Try to leave current room.
        Room nextRoom = currentRoom.getExit(direction);
        if(currentRoom == transport)
        {
            nextRoom = transport.getExit(direction);
        }

        if (nextRoom == null) 
        {
            System.out.println("There is no door!");
        }
        // for TransporterRoom

        else 
        {
            previousRoom = currentRoom; // store the previous room
            previousRoomStack.push(currentRoom); // and add to previous room stack
            currentRoom = nextRoom;
            System.out.println(currentRoom.getLongDescription());
        }
    }

    /**
     * Look lets you know where the user is at the moment
     * 
     * @param command is the user input which allows him to look where he is 
     * at the moment
     */
    private void look(Command command)
    {
        if(command.hasSecondWord())
        {
            System.out.println("Look what?");
        }
        else{
            System.out.println(currentRoom.getLongDescription());
            if(currItem != null)
            {
                System.out.println("You have:" + currItem.itemName() + "currently");
            }
            else
            {
                System.out.println("You are carrying no items currently");
            }   
        }
    }

    /** 
     * "Eat" was entered. Check the rest of the command to see
     * whether we really want to eat.
     * 
     * @param command The command to be processed
     */
    private void eat(Command command) 
    {
        if(command.hasSecondWord()){
            System.out.println("Eat what?");
        }
        else{
            if(currItem == null)
            {
                System.out.println("You have no food and need energy");
            }
            else{
                if(currItem.itemName().equals("candy"))
                {
                    System.out.println("You have eaten a candy and are now full");
                    currItem = null;
                    energy = 0;
                }

                else{
                    System.out.println("You are not hungry as you have eaten");
                }
            }
        }
    }

    /** 
     * "Back" was entered. Check the rest of the command to see
     * whether we really quit the game.
     * 
     * @param command The command to be processed
     */
    private void back(Command command) 
    {
        if(command.hasSecondWord()) {
            System.out.println("Back what?");
        }
        else {
            // go back to the previous room, if possible
            if (previousRoom==null) {
                System.out.println("No room to go back to.");
            } else {
                // go back and swap previous and current rooms,
                // and put current room on previous room stack
                Room temp = currentRoom;
                currentRoom = previousRoom;
                previousRoom = temp;
                previousRoomStack.push(temp);
                // and print description
                System.out.println(currentRoom.getLongDescription());
            }
        }
    }

    /** 
     * "StackBack" was entered. Check the rest of the command to see
     * whether we really want to stackBack.
     * 
     * @param command The command to be processed
     */
    private void stackBack(Command command) 
    {
        if(command.hasSecondWord()) {
            System.out.println("StackBack what?");
        }
        else {
            // step back one room in our stack of rooms history, if possible
            if (previousRoomStack.isEmpty()) {
                System.out.println("No room to go stack back to.");
            } else {
                //current room becomes previous room, and
                //current room is taken from the top of the stack
                previousRoom = currentRoom;
                currentRoom = previousRoomStack.pop();
                //and print description
                System.out.println(currentRoom.getLongDescription());
            }
        }
    }

    /**
     * This method lets the player drop an item in 
     * the room the player is at currently
     * 
     *@param command the command to be processed which is 
     *allows the user to drop an item
     */
    private void drop(Command command)
    {
        if(command.hasSecondWord())
        {
            System.out.println("Drop what?");
        }
        else{
            if(currItem==null){
                System.out.println("You have no item to drop");
            }
            else if(currItem.itemName() != null){
                System.out.println("You dropped:" + currItem.itemName());
                currentRoom.addItem(currItem);
                currItem = null;
            }
        }    
    }

    /**
     * This method allows the user to take any item
     * present in different locations
     * 
     * @param command the command to be processed 
     * allows the user to take an item
     */
    private void take(Command command)
    {
        String item = command.getSecondWord();

        Item tempItem = new Item(item, currentRoom.itemWeight(item), item);
        if(!command.hasSecondWord())
        {
            System.out.println("Take what?");
            return;
        }

        else{

            if(energy < 5 && item.equals("beamer") &&  currentRoom.itemHere(item) && currItem == null)
            {
                System.out.println(" You are holding the beamer");
                currBeamer = new Beamer("a weapon", currentRoom.itemWeight(item), "beamer");
                currItem = tempItem;
                currentRoom.removeItem(currItem);
                return;
            }

            if( energy < 5 && currentRoom.itemHere(item) && currItem == null)
            {
                System.out.println("You have an item:" + item);
                currItem = tempItem;
                currentRoom.removeItem(currItem);
                energy++;
                return;
            }
            if(item.equals("candy")&& currentRoom.itemHere(item) && currItem == null)

            {
                System.out.println("You picked up a candy");
                currItem = tempItem;
                currentRoom.removeItem(currItem);
                return;
            }
            else if(currItem != null)
            {
                System.out.println("You are holding something else");
            }
            else if(!currentRoom.itemHere(item))
            {
                System.out.println("The item you are looking for in not in this room");
            }
            else if(energy == 5 && !item.equals("candy"))
            {
                System.out.println("You must eat before picking something because you are hungry");
            }
        }
    }

      
    /**
     * This method allows the user to charge the beamer
     * 
     * @param command allows the command to be processed which is charges the beamer
     */
    private void charge(Command command)
    {
        if(command.hasSecondWord())
        {
            System.out.println("Charge what?");
            return;
        }
        else{
            if(currBeamer.isChargedBeamer())
            {
                System.out.println("Beamer was charged already");
                return;
            }
            if( !currItem.itemName().equals("beamer") ||currItem == null)
            {
                System.out.println("You have no beamer");
                return;
            }

            else if(!currBeamer.isChargedBeamer() && currItem.itemName().equals("beamer") )
            {
                System.out.println("Beamer is charged now");
                currBeamer.isBeamer(currentRoom);
            }

        }
    }

    /**
     * This method allows the user to fire the beamer after it has been charged
     * 
     * @param command allows the command to be processed which is fire the beamer when charged
     */

    private void fire(Command command)
    {
        if (command.hasSecondWord()){
            System.out.println("Fire what?");
        }
        else{
            if (!currBeamer.isChargedBeamer())
            {
                System.out.println("You have to charge your beamer first");
            }
            else
            {
                
                currBeamer.isFiredBeamer();
                previousRoom = currentRoom;
                currentRoom = currBeamer.getRoomBeamer();
                System.out.println(currentRoom.getLongDescription());
                
                previousRoomStack.push(previousRoom);
                currItem = null;
            }
        }
    }

    /** 
     * "Quit" was entered. Check the rest of the command to see
     * whether we really quit the game.
     * 
     * @param command The command to be processed
     * @return true, if this command quits the game, false otherwise
     */
    private boolean quit(Command command) 
    {
        if(command.hasSecondWord()) 
        {
            System.out.println("Quit what?");
            return false;
        }
        else {
            return true;  // signal that we want to quit
        }
    }
}

/**
 * This class represents an item which may be put
 * in a room in the game of Zuul.
 * 
 * @author Lynn Marshall 
 * @version A3 Solution
 * 
 * @author Shoana Sharma
 * @version Novermber.12.2017
 * 
 * Using Professor Marshall's solution from assignment 3
 */
public class Item
{
    // description of the item
    private String description;
    private String name;
    // weight of the item in kilgrams 
    private double weight;

    /**
     * Constructor for objects of class Item.
     * 
     * @param description The description of the item
     * @param weight The weight of the item
     */
    public Item(String description, double weight,String name)
    {
        this.description = description;
        this.weight = weight;
        this.name = name;
    }

    /**
     * Returns a description of the item, including its
     * description and weight.
     * 
     * @return  A description of the item
     */
    public String getDescription()
    {
        return description + " that weighs " + weight + "kg.";
    }

    /**
     * This method gives the name of the item
     * @return the name of the item 
     */
    public String itemName()
    {
        return name;
    }

    /**
     * This method allows the user to get the weight of an item 
     * @return the weight of the item
     */
    public double weightItem()
    {
        return weight;
    }
}

import java.util.Scanner;
import java.util.StringTokenizer;

/**
 * This class is part of the "World of Zuul" application. 
 * "World of Zuul" is a very simple, text based adventure game.  
 * 
 * This parser reads user input and tries to interpret it as an "Adventure"
 * command. Every time it is called it reads a line from the terminal and
 * tries to interpret the line as a two word command. It returns the command
 * as an object of class Command.
 *
 * The parser has a set of known command words. It checks user input against
 * the known commands, and if the input is not one of the known commands, it
 * returns a command object that is marked as an unknown command.
 *  
 * @author  Michael Kolling and David J. Barnes
 * @version 2006.03.30
 * 
 * @author Lynn Marshall
 * @version A3 Solution
 * 
 * @author Shoana Sharma
 * @version Novermber.12.2017
 * 
 * Using Professor Marshall's solution from assignment 3
 */
public class Parser 
{
    private CommandWords commands;  // holds all valid command words
    private Scanner reader;         // source of command input

    /**
     * Create a parser to read from the terminal window.
     */
    public Parser() 
    {
        commands = new CommandWords();
        reader = new Scanner(System.in);
    }

    /**
     * Get the command from the user.
     * 
     * @return The next command from the user
     */
    public Command getCommand() 
    {
        String inputLine;   // will hold the full input line
        String word1 = null;
        String word2 = null;

        System.out.print("> ");     // print prompt

        inputLine = reader.nextLine();

        // Find up to two words on the line.
        Scanner tokenizer = new Scanner(inputLine);
        if(tokenizer.hasNext()) {
            word1 = tokenizer.next();      // get first word
            if(tokenizer.hasNext()) {
                word2 = tokenizer.next();      // get second word
                // note: we just ignore the rest of the input line.
            }
        }

        // Now check whether this word is known. If so, create a command
        // with it. If not, create a "null" command (for unknown command).
        if(commands.isCommand(word1)) {
            return new Command(word1, word2);
        }
        else {
            return new Command(null, word2); 
        }
    }

    /**
     * Get a list of valid command words.
     * 
     * @return A String of the valid commands
     */
    public String getCommands()
    {
        return commands.getCommandList();
    }
}
import java.util.*;// or java.util.*; and replace the above

/**
 * Class Room - a room in an adventure game.
 *
 * This class is part of the "World of Zuul" application. 
 * "World of Zuul" is a very simple, text based adventure game.  
 *
 * A "Room" represents one location in the scenery of the game.  It is 
 * connected to other rooms via exits.  For each existing exit, the room 
 * stores a reference to the neighboring room.
 * 
 * @author  Michael Kolling and David J. Barnes
 * @version 2006.03.30
 * 
 * @author Lynn Marshall 
 * @version A3 Solution
 * 
 * @author Shoana Sharma
 * @version Novermber.12.2017
 * 
 * Using Professor Marshall's solution from assignment 3
 */

public class Room 
{
    private String description;
    private HashMap<String, Room> exits;        // stores exits of this room.

    // the items in this room
    private ArrayList<Item> items;

    /**
     * Create a room described "description". Initially, it has
     * no exits. "description" is something like "a kitchen" or
     * "an open court yard".
     * 
     * @param description The room's description.
     */
    public Room(String description) 
    {
        this.description = description;
        exits = new HashMap<String, Room>();
        items = new ArrayList<Item>();
    }

    /**
     * Add an item to the room, best to check that it's not null.
     * 
     * @param item The item to add to the room
     */
    public void addItem(Item item) 
    {
        if (item!=null) { // not required, but good practice
            items.add(item);
        }
    }

    /**
     * Define an exit from this room.
     * 
     * @param direction The direction of the exit
     * @param neighbour The room to which the exit leads
     */
    public void setExit(String direction, Room neighbour) 
    {
        exits.put(direction, neighbour);
    }

    /**
     * Returns a short description of the room, i.e. the one that
     * was defined in the constructor
     * 
     * @return The short description of the room
     */
    public String getShortDescription()
    {
        return description;
    }

    /**
     * Return a long description of the room in the form:
     *     You are in the kitchen.
     *     Exits: north west
     *     Items: 
     *        a chair weighing 5 kgs.
     *        a table weighing 10 kgs.
     *     
     * @return A long description of this room
     */
    public String getLongDescription()
    {
        return "You are " + description + ".\n" + getExitString()
        + "\nItems:" + getItems();
    }

    /**
     * Return a string describing the room's exits, for example
     * "Exits: north west".
     * 
     * @return Details of the room's exits
     */
    private String getExitString()
    {
        String returnString = "Exits:";
        Set<String> keys = exits.keySet();
        for(String exit : keys) {
            returnString += " " + exit;
        }
        return returnString;
    }

    /**
     * Return the room that is reached if we go from this room in direction
     * "direction". If there is no room in that direction, return null.
     * 
     * @param direction The exit's direction
     * @return The room in the given direction
     */
    public Room getExit(String direction) 
    {
        return exits.get(direction);
    }

    /**
     * Return a String representing the items in the room, one per line.
     * 
     * @return A String of the items, one per line
     */
    public String getItems() 
    {
        // let's use a StringBuilder (not required)
        StringBuilder s = new StringBuilder();
        for (Item i : items) {
            s.append("\n    " + i.getDescription());
        }
        return s.toString(); 
    }

    /**
     * This method removes the item from the current room
     * @param the item removes the item from the arrayList
     */
    public void removeItem(Item item)
    {
        Iterator<Item> iter = items.iterator();
        while(iter.hasNext())
        {
            Item n = iter.next();

            if(item.itemName().equals(n.itemName()))
            {
                iter.remove();
            }

        }
    }

    /**
     * This method returns a boolean if the item is present
     * in the present in the current room
     * 
     * @param item which is the name of the item
     * @return true if the item inputed by the user is present 
     * otherwise return false
     */
    public boolean itemHere(String item)
    {
        for (Item n: items)
        {
            if (item.equals(n.itemName()))
            {
                return true;
            }

        }
        return false;
    }

    /**
     * This method check the item's weight
     * 
     * @param item is the weight of the item
     * @return the weight of the item in a double.
     */
    public double itemWeight(String item)
    {
        double count = 0;
        for (Item n: items)
        {
            if (item.equals(n.itemName()))
            {
                count +=n.weightItem();
            }
        }
        return count;
    }
}

import java.util.*;
/**
 * A "Transported" represents one location in the game.  It randomly takes you to any other room present in the game. 
 * 
 * @author Shoana Sharma
 * @version Novermber.12.2017
 */

public class TransporterRoom extends Room
{

    private Random r;
    private int i;
    private ArrayList<Room> room;
    /**
     * Contructor for Transporter room as given in the assignment package
     * 
     * @param decription of the room as TransporterRoom is the sub-class of room
     * @param rooms is an ArrayList of room
     */
    
    public TransporterRoom(String description,ArrayList<Room> rooms)
    {
        super(description);
        room = rooms;
        this.i = i;
    }
    
    /**
     * Returns a random room, independent of the direction parameter.
     *
     * @param direction Ignored.
     * @return A randomly selected room.
     */
    public Room getExit(String direction)
    {
        return findRandomRoom();
    }

    /**
     * Choose a random room.
     *
     * @return The room we end up in upon leaving this one.
     */
    private Room findRandomRoom()
    {
        Random x = new Random();
        i = x.nextInt(room.size());
        return room.get(i);
    }
}
