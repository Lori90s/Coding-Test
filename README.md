# Coding-Test


Question 1
Write a method that returns the sum of the first 100 even numbers of the Fibonacci sequence.

class SumEvenFibonacci {
    public static void main(String [] args){
                int f1=0;
        int f2=0;
        int f3=1;
        int sum=0; //the result

        for(int i=0;i<100;i++){

            /*
            Calculate Fibonacci numbers
            */
            f1=f2;
            f2=f3;
            f3=f1+f2;
            
            //if this is the even number
            if(f3%2==0){
                sum = sum+f3;
            }else{
                i=i-1; // find the next even Fibonacci number
            }
        }

        // the sum should be 431797724

        System.out.println("The sum of the first 100 even numbers of the Fibonacci sequenc is " + sum);
        
    }
}

Question 2
A palindromic number reads the same both ways. The largest palindrome made from the product of two 2-digit numbers is 9009 = 91 × 99.
    
Write a method that finds and returns the largest palindrome made from the product of two 3-digit numbers as well as the two 3-digit numbers.

package com.company;

public class Main {

    // check if the number is palindrome 

    public static boolean isPalindrome(int num){

        int originalNum = num;


        StringBuilder sb1 = new StringBuilder(""+originalNum);
        String sb2 = ""+originalNum;
        sb1.reverse();
        return sb2.equals(sb1.toString());

    }

    public static void main(String[] args) {
        // write your code here

        int d1=0;
        int d2=0;
        int p=0;
        int max=0; // the largest palindrome
        int dm1=0; // the first 3-digit numbers producing the largest palindrome
        int dm2=0; // the second 3-digit numbers producing the largest palindrome

        for(d1=100;d1<1000;d1++){
            for(d2=100;d2<1000;d2++){
                p=d1*d2;
                if(p>max && (isPalindrome(p))){
                    max=p;
                    dm1=d1;
                    dm2=d2;
                }

                }
        }

        // the result should be 906609=913*993

        System.out.println("the largest palindrome made from the product of two 3-digit numbers is :  ");

        System.out.println( max + "=" + dm1 + "*" + dm2);

    }

}

Question 3
Implement a queue with methods void Enqueue(T) and T Dequeue() with the following conditions
1. Your queue may only use stacks as member variables. You can assume a stack has the following methods void Push(T), T Pop(), int Size()
2. The Dequeue method must be O(1)

// this is the code found from https://stackoverflow.com/questions/69192/how-to-implement-a-queue-using-two-stacks

public class MyStack<T> {

    // inner generic Node class
    private class Node<T> {
        T data;
        Node<T> next;

        public Node(T data) {
            this.data = data;
        }
    }

    private Node<T> head;
    private int size;

    public void push(T e) {
        Node<T> newElem = new Node(e);

        if(head == null) {
            head = newElem;
        } else {
            newElem.next = head;
            head = newElem;     // new elem on the top of the stack
        }

        size++;
    }

    public T pop() {
        if(head == null)
            return null;

        T elem = head.data;
        head = head.next;   // top of the stack is head.next

        size--;

        return elem;
    }

    public int size() {
        return size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public void printStack() {
        System.out.print("Stack: ");

        if(size == 0)
            System.out.print("Empty !");
        else
            for(Node<T> temp = head; temp != null; temp = temp.next)
                System.out.printf("%s ", temp.data);

        System.out.printf("\n");
    }
}

public class MyQueue<T> {

    private MyStack<T> inputStack;      // for enqueue
    private MyStack<T> outputStack;     // for dequeue
    private int size;

    public MyQueue() {
        inputStack = new MyStack<>();
        outputStack = new MyStack<>();
    }

    public void enqueue(T e) {
        inputStack.push(e);
        size++;
    }

    public T dequeue() {
        // fill out all the Input if output stack is empty
        if(outputStack.isEmpty())
            while(!inputStack.isEmpty())
                outputStack.push(inputStack.pop());

        T temp = null;
        if(!outputStack.isEmpty()) {
            temp = outputStack.pop();
            size--;
        }

        return temp;
    }

    public int size() {
        return size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

}

Question 4
Model a standard deck of cards for use in a variety of card games (Poker, Blackjack, etc).

Consider the following areas.
1) How would you create the deck?
2) How would you shuffle the deck?
3) How would you deal cards?
4) How could you determine a winning hand?


import java.util.List;

//Model a single Card
public class Card {

    private int rank, suit;

    private static String[] suits = { "hearts", "spades", "diamonds", "clubs" };
    private static String[] ranks  = { "Ace", "2", "3", "4", "5", "6", "7", "8", "9", "10", "Jack", "Queen", "King" };

    Card(int suit, int rank){
        this.rank=rank;
        this.suit=suit;
    }

    @Override
    public String toString(){
        return ranks[rank] + " of " + suits[suit];
    }

    public int getRank() {
        return rank;
    }

    public int getSuit() {
        return suit;
    }

}

/* model a deck
    1: create a deck  - use arraylist to store the cards in sequence by using loops
    2: shuffle a deck - randomly generate two position numbers and exchange the two cards in a deck
    3: take out the first card in a deck when cards are being distributed
    4: return the number of cards to see if dealing is done
*/

public class Deck {

    private ArrayList<Card> cards;


    //create a deck in sequence
    public Deck(){

        cards = new ArrayList<Card>();

        //suit
        for (int a=0; a<=3; a++){
            //rank
            for (int b=0; b<=12; b++)
            {
                cards.add( new Card(a,b) );
            }
        }

    }

    // shuffle a deck
    public ArrayList<Card> shuffle(){

        int index_1, index_2;
        Random generator = new Random();
        Card temp;

        // randomly exchange two cards in the deck
        for (int i=0; i<100; i++){

            index_1 = generator.nextInt( cards.size() - 1 );
            index_2 = generator.nextInt( cards.size() - 1 );

            temp = (Card) cards.get( index_2 );
            cards.set( index_2 , cards.get( index_1 ) );
            cards.set( index_1, temp );
        }

        return cards;
    }

    //always take out the first card in a deck when a player is dealing cards
    public Card drawFromDeck()
    {
        return cards.remove( 0 );
    }

    // return the number of cards left in the deck to know if card distribution has been done
    public int getTotalCards(){
        return cards.size();
    }

}


class Player {

    private int num = 0;

    //get one card each time from the dealer
    public void getCard(){
        num = num +1;
    }

    //remove a certain number of cards when starting the game
    public void playCard(int m){
        num = num -m;
    }

    //check if the player has cards left or not
    public boolean emptyHand(){
        return (num==0);
    }

}

//dealing cards 
class Dealer {

    public void deal(List<Player> player, Deck deck) {
        // give each player cards

        List<Player> players = player;
        Deck deck0 = deck;

        //shuffle cards before dealing
        deck0.shuffle();

        // distribute cards to each person when the cards left in the deck is more than the number of players
        while(!(deck0.getTotalCards() < players.length)){

            for (int i =0; i<players.length; i++){
                Player t = players.get(i);
                t.getCard();
                deck0.drawFromDeck();

            }
        }
    }


}


// initailize the game - dealer, deck and players
// determine a winning hand
class Table {
    private Dealer tDealer;
    private List<Player> player;
    private Deck tDeck;

    Table(int numPlayers){

        tDeck = new Deck();

        tDealer = new Dealer();

        for(int i = 0; i < numPlayers ; i ++){
            player.add(new Player());
        }

    }

    // who get rid of all cards in hand first wins the game
    public Player winningHand(){
        for(int i = 0; i < numPlayers ; i ++){
            if(player.get(i).emptyHand()){
                return player.get(i);
            }
        }

        return null;
    }

}


Question 5
You are tasked with finding all valid email addresses found within all files within a directory structure.

Explain how you would do this and include any scripts or programs you would use.


Steps:

1. check if next path has a valid file
2. if it is a file, then use file_to_str(filename) to transfer all contens into one string 
3. find and print all valid email address in that string one by one 
4. if there are more pathes to check, go to step 1
5. othewise exit 




Scripts: 
    
#!/usr/bin/env python
#
# Extracts email addresses from one or more plain text files.
#
# Notes:
# - Does not save to file (pipe the output to a file if you want it saved).
# - Does not check for duplicates (which can easily be done in the terminal).
#
# (c) 2013  Dennis Ideler <ideler.dennis@gmail.com>

from optparse import OptionParser
import os.path
import re

regex = re.compile(("([a-z0-9!#$%&'*+\/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+\/=?^_`"
                    "{|}~-]+)*(@|\sat\s)(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?(\.|"
                    "\sdot\s))+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?)"))

def file_to_str(filename):
    """Returns the contents of filename as a string."""
    with open(filename) as f:
        return f.read().lower() # Case is lowered to prevent regex mismatches.

def get_emails(s):
    """Returns an iterator of matched emails found in string s."""
    # Removing lines that start with '//' because the regular expression
    # mistakenly matches patterns like 'http://foo@bar.com' as '//foo@bar.com'.
    return (email[0] for email in re.findall(regex, s) if not email[0].startswith('//'))

if __name__ == '__main__':
    parser = OptionParser(usage="Usage: python %prog [FILE]...")
    # No options added yet. Add them here if you ever need them.
    options, args = parser.parse_args()

    if not args:
        parser.print_usage()
        exit(1)

    for arg in args:
        if os.path.isfile(arg):
            for email in get_emails(file_to_str(arg)):
                print email
        else:
            print '"{}" is not a file.'.format(arg)
            parser.print_usage()
