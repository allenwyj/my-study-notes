# Java Codes

Random Number

```java
import java.lang.Math;

// Method 1:
public static double getRandomDoubleBetweenRange(double min, double max){
    double x = (Math.random() * ((max - min) + 1)) + min;
    return x;
}

// Method 2:
import java.util.Random;

// For getting random Index
public static int generateRandomIntWithRange(int index) {
    // create a random number generator
		Random r = new Random();
    return r.nextInt(index);
}

// Within [min, max]
public static int generateRandomIntWithRange(int min, int max) {
    // create a random number generator
		Random r = new Random();
    return r.nextInt((max - min) + 1) + min;
}

// Within [0, max]
public static int generateRandomInt(int max) {
    // create a random number generator
		Random r = new Random();
    return r.nextInt(max + 1);
}

// Within [1, max]
public static int generateRandomInt(int max) {
    // create a random number generator
		Random r = new Random();
    return r.nextInt(max) + 1;
}
```

Read file

```java
import java.io.*;
import java.util.Scanner;

/**
 * Read file by using Scanner .
*/
public void readFromFile()
{
	System.out.println("Reading file...");
	String filename = "teams.txt";
	try
	{
		FileReader input = new FileReader(filename);
		try
		{
			Scanner read = new Scanner(input);
			while (read.hasNextLine())
			{
				String[] stringGroup = new String [2];
				String readLine = read.nextLine();
				stringGroup = readLine.split(",");
				String teamName = stringGroup[0];
				String teamRanking = stringGroup[1];
				int ranking = convertToString(teamRanking);
				addTeam(teamName, ranking);
			}
		}
		finally
		{
			System.out.println("Finished. Closing file...");
			System.out.println(" ");
			input.close();
		}
	}
	catch(FileNotFoundException exception)
	{
		System.out.println("The " + filename + " does not exist.");
	}
	catch(IOException exception)
	{
		System.out.println("## Unexpected I/O error occured. ##");
	}
}

// using BufferedReader 
import java.io.BufferedReader;
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class ReadFromFile2 
{ 
  public void readFromFile() { 
	  // We need to provide file path as the parameter: 
	  // double backquote is to avoid compiler interpret words 
	  // like \test as \t (ie. as a escape sequence) 
	  File file = new File("C:\\Users\\allen\\Desktop\\test.txt"); 
	  
		System.out.println("Reading fortunes from file: " + file);
		System.out.println("File exists: " + file.exists());
	
		try (BufferedReader br = new BufferedReader(new FileReader(file))) {
			String tempLine; 
		  while ((tempLine = br.readLine()) != null) {
		    System.out.println(tempLine);
			}
	  } catch (IOException e) {
				e.printStackTrace();
		}
	}
}
```

The try-with-resources statement is a try statement that declares one or more resources. A resource is an object that must be closed after the program is finished with it. The try-with-resources statement ensures that each resource is closed at the end of the statement. e.g. **BufferedReader**. It will be closed regardless of whether the try statement completes normally or abruptly

Output File

```java
public void writeFileOutput()
{
    String outputFileName = "statistics.txt";
    try
    {
        PrintWriter outputFile = new PrintWriter(outputFileName);
        outputFile.println(" ============================================================================================ ");
        outputFile.println("| Football World Cup Winner: " + teams.get(0).getName() + " !!!!!!!                          |");
        outputFile.println("|>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>|");
        outputFile.println(" ******************************** Golden Boot Award ******************************** ");
        outputFile.println(" " + getGoldenBoots());
        outputFile.println(" ******************************** Fair Play Award ******************************** "); 
        outputFile.println(" " + getFairPlayCountry());
        outputFile.close();
    }
    catch(IOException e)
    {
        System.out.println("## Error! You don't have permission to store the cup result. ##");
    }
}
```