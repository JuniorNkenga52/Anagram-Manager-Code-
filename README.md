# Anagram-Manager-Code-
import java.util.*;
/**The AnagramManager Class is the template for the 
*creation of the AnagramManager objects.
*It has all the fields and methods of the objects of type
*AnagramManager.
*@author Nkengla Muluh Awa Junior
*@version 05/13/2018
*/
public class AnagramManager {
 private Word[] array;
 /**Constructor for the AnagramManager class that initialises the fields of the AnagramManager object 
 *@param list the dictionary of words to be used for the program
 *@throws IllegalArgumentException if the list is empty
 */
 public AnagramManager(List<String> list){
 if(list.size()==0) throw new IllegalArgumentException();
 array=new Word[list.size()];//the array field size is now the size of the list.
 for(int i=0; i<list.size(); i++){ // going through the list which is the dictionary of words
  Word add=new Word(list.get(i));// creating a new Word Object
  array[i]=add; //the Array at that index now points to the Word object created.
  
  }
 }
 /**Sorts the internal array based on the Original or actual word value.
 */
 public void sortByWord(){
 sortByWord(array);
 }
 /**Sorts the internal array based on the Canonical word value.
 */
 public void sortByForm(){
 List<Word> myList=new ArrayList<Word>(Arrays.asList(array));//creating a new Array List whose elements are those of the array so that the compareTo method can be used
 Collections.sort(myList);//sorting the List on the basis of their Canonical Words.
 array=myList.toArray(array);//now making the array point to the sorted list
 }
 private void sortByWord(Word[] a){
  if(a.length>=2){
 Word[] one=Arrays.copyOfRange(a,0,a.length/2);
 Word[] two=Arrays.copyOfRange(a,a.length/2,a.length);
  sortByWord(one);
  sortByWord(two);
  merge(a,one,two);
  }
 }
 private void merge(Word[] origin,Word[] one,Word[] two){
  int i1 = 0;   // index into left array    
   int i2 = 0;   // index into right array   
   //going through theList and replacing items based on which is less
    for (int i = 0; i <origin.length; i++) { 
       // checking to see if the word from the left array is less than that from the right array 
       //Also checking to see if the index of the right array is greater than the its size
       //in this case, the person to be put in that position is taken from the left array
       if (i2 >= two.length ||  (i1 < one.length && one[i1].getWord().compareTo(two[i2].getWord())<0)){       
            origin[i]=one[i1];   // replacing the word object at given index in the original array with word object from the first array.        
            i1++;                       // updating index of the left array.
        } else {            
        origin[i]=two[i2];   //replacing the person at given index in the original array with word object from the second array.
            i2++;                   //updating index of the right array.
          }    
    }
}
/**Returns a random word in the array whose canonical form is the same as that of the string input into the method
*@param x the String input by the user.
*@return the random word with the same canonical form or an empty String no word has the same canonical form
*/
 public String getAnagram(String x){
  x=canon(x);//now making the string point to its canonical form
 Random rand=new Random();//creating the Random object generator
List<String> myList=new ArrayList<>();//creating a new list to hold all the words with same canonical form as the String input by user
 for(int i=0; i<array.length; i++){ //going through the array
 if(array[i].getForm().equals(x)) myList.add(array[i].getWord()); //adding the word to the list if it has the same canonical form 
 }
 if(!myList.isEmpty()) return myList.get(rand.nextInt(myList.size()));//randomly selecting a word from the list if the list is not empty
return ""; //returning an empty string if no word has the same canonical form
}
/**returns the set of all the words having the same canonical form as the word input by the user
*@param x the String which is input the user
*@return the set of all the words with the same canonical form
*/
  public Set<String> getAnagrams(String x){
  x=canon(x);// the word input by the user now points to its canonical form
 Set<String> mySet=new TreeSet<>();//creating a new set to hold all the words with the same canonical form as that input by the user
 for(int i=0; i<array.length; i++){//going through the array
 if(array[i].getForm().equals(x)) mySet.add(array[i].getWord()); //adding the word to the set if its has same canonical form
 }
 return mySet; // sending the set back to the caller
}
/**returns the first five and last five Word objects in the array field
@return th five and last five word Objects in the array field
*/
    public String toString(){
     String result="";//creating an empty string
     for(int i=0; i<5; i++) result+=array[i].toString();//adding the first five elements to the empty string
     result+="[...]";//separating the first five elements from the last five
     for(int j=array.length-5; j<array.length; j++)result+=array[j].toString();// adding the last five elements to the first five string
      return result;//returning the concatenated String of the first and last five Word Objects in the array to the caller
    }
   private String canon(String a){
   char[] c=a.toLowerCase().toCharArray();// converting the string to an array of type char
 Arrays.sort(c);//sorting the array based on its characters
 return String.valueOf(c);//now making the string point to its canonical form 
  }
 }
