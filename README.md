
package linked;


import javafx.application.Application;
import java.util.Scanner; 
import java.io.*;
import java.util.LinkedList;

public class Linked {

    public static void main(String[] args) {
       LinkList myLinkList = new LinkList();

  
        System.out.println("--------------------------");
        
        //Let's populate the list with sample stuff...
        System.out.println("inserting elements...");

        myLinkList.listInsert(new trainUserTyped("Lechmere"));
        myLinkList.listInsert(new trainUserTyped("Copley"));
        myLinkList.listInsert(new trainUserTyped("Hynes"));
        myLinkList.listInsert(new trainUserTyped("Pru"));
        
      

        System.out.println("Now the list is:");
        myLinkList.printList();
        System.out.println("----------------------");

       //Delete a name
       //Again, same routine, all this stuff is happening before you even type into the program.
        myLinkList.listDelete(new trainUserTyped("Hynes"));

        System.out.println("After deleting Hynes, the list is:");
     
//Print updated list
        myLinkList.printList();
        System.out.println("----------------------");
        
        
        
        
        
        
        //Now we get out of the simulation and begin using real input
        Scanner scan = new Scanner(System.in);
     
        System.out.println(""
                + "Menu\n "
                + "1 = Add something to Front \n "
                + "2 = Add something to End\n "
                + "3 = Remove Something By Node Number\n "
                + "4 = Print Current List\n" );  
        
        int answer=scan.nextInt();  
        
        if (answer == 1)  //Add to the front
        {
        // codes
         System.out.println("What stop do you want to add to the front?" );  
         String train=scan.next();  
         myLinkList.addToFront( new trainUserTyped(train)); 
         
        System.out.println("Now the list is:");
        myLinkList.printList();
        System.out.println("----------------------");
        }
        
        else if(answer == 2) //Add to the back
        {
         System.out.println("What stop do you want to add to the back?" );  
         String train=scan.next();  
         myLinkList.addToBack( new trainUserTyped(train));
         
        System.out.println("Now the list is:");
        myLinkList.printList();
        System.out.println("----------------------");
        }

        else if (answer == 3) //Remove by node number
        {
         System.out.println("What train number do you want to remove?" ); 
         int position=scan.nextInt();   
         myLinkList.removeByNodeNumber(position);
         System.out.println("Now the list is:");
         myLinkList.printList();
        }
        
        else
        {
            System.out.println("The Current List Is:");
            myLinkList.printList();
        }

    }
        
}




//This is the class that holds and formats the user's input to be something we can use
class trainUserTyped {
    String train;

    public trainUserTyped(String first) {
        this.train = first;

    }

    public String toString() {
        return train ;
    }    
}



class NODE {

    trainUserTyped info;
    NODE link;
    

    public NODE(trainUserTyped info) {
        this.info = info;
    }
//Dummy node
    public NODE() {
        info = new trainUserTyped("\0"); 
    }
    
}


//**************THIS IS THE ACTUAL LINKED LIST********************
class LinkList{

    NODE Node; //Dummy node

    LinkList() {  //Think of this as a 1950's phone tree.
        //initialize Node
        Node = new NODE();
        Node.link = null; //So right now, Node's card isn't telling you to go anywhere.
    }

    public void listInsert(trainUserTyped newtrainUserTyped) {
        NODE last, next;
        next = Node; 
        last = null;

        while ((newtrainUserTyped.train.compareTo(next.info.train) > 0) && (next.link != null)) {
            last = next;
            next = next.link;
        }

        //This is the part that actually sorts the list
        //If there's no difference....
        if (newtrainUserTyped.train.compareTo(next.info.train) == 0) {
            next.info = newtrainUserTyped;
            //Just make the next node whatever the user typed.
            
        //If they're different....
        } else if (newtrainUserTyped.train.compareTo(next.info.train) < 0) {
            last.link = new NODE();
            //...Squeeze the new node in before the next one
            last.link.info = newtrainUserTyped;
            last.link.link = next;
        } else {
            NODE temp = next.link; 
            next.link = new NODE();   
            next.link.info = newtrainUserTyped;
            next.link.link = temp;
        }

    }
   
    
    public NODE listDelete(trainUserTyped part) {
        NODE last, next;
        next = Node; 
        last = null;

        while (!part.train.equals(next.info.train) && next.link != null) {
            last = next;
            next = next.link;
        }

        if (part.train.equals(next.info.train)) {
            last.link = next.link;
            return next; //free the next node
        }

        return new NODE();

    }
    
    
    public void printList() {
      
	   NODE next = Node;

        int num = 1;
        
        //while next actually has something in it
        while (next != null) {
            
            //skip printing the empty header node
            if (next == Node) {
                next = next.link;
                continue;
            }
            //print out each element with its number
            System.out.println("["+num+"]"+next.info);
            num++;
            next = next.link;
        }

    }
    
    
    
    
    
    
    
    
      //This function adds to the front of the list
    public void addToFront(trainUserTyped newtrainUserTyped){  //You want to add a string called "train" to the front of the list
        
        //trainUserTyped user_input = new trainUserTyped(train); //To do that, you first have to cast train
                                                               //to a different type of data called trainUserTyped, 
                                                               //then throw that away into cyberspace because it's garbage.
                                                               //Now, you've converted that same data into a form you can
                                                               //actually use, called user_input.
        
        NODE freshlyBakedNode = new NODE(); //Here, we're NOT casting because we actually put user_input in the node.
        
          freshlyBakedNode.link = Node.link; //Now create a new node equal to where the list began.
          freshlyBakedNode.info = newtrainUserTyped;
         Node.link = freshlyBakedNode; //Now Node tells you "nope, it's not me, go to FBN instead."
    }
        
    
    
    //This function adds to the back of the list  
    public void addToBack(trainUserTyped newtrainUserTyped) {
        NODE last, next;
        next = Node;
        last = null;

        while ((newtrainUserTyped.train.compareTo(next.info.train) > 0) && (next.link != null)) {
            last = next;
            next = next.link;
        }

        //This is the part that actually sorts the list
        //If there's no difference....
        if (newtrainUserTyped.train.compareTo(next.info.train) == 0) {
            next.info = newtrainUserTyped;
            //Just make the next node whatever the user typed.
            
        //If they're different....
        } else if (newtrainUserTyped.train.compareTo(next.info.train) < 0) {
            last.link = new NODE();
            //...Squeeze the new node in before the next one
            last.link.info = newtrainUserTyped;
            last.link.link = next;
        } else {
            NODE temp = next.link; //
            next.link = new NODE();     
            next.link.info = newtrainUserTyped;
            next.link.link = temp;
        }
       
    }

    public void removeByNodeNumber(int position)  {
        
   NODE currentNode = Node; //This is set to Node because he comes first
   NODE previousNode = null; //null because you haven't done anything yet

   int count = 0;
        
    while (currentNode != null)      //meaning while the next thing actually has something in it
    {//Keep going to the next
        if (position == 1)
        {
            Node = currentNode.link;
            break;
        }
        else if(count == position){
            previousNode.link = currentNode.link;
        
        }else{
            previousNode = currentNode;
            currentNode = currentNode.link;
            
        }
        count++;
    }
    }
}
     
