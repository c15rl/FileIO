# FileIO
CS3 assignment (Dalton)
import java.util.*;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.Writer;
import java.net.URL;
import java.io.FileNotFoundException;

public class FileIO 
{
	static Student TwinA;
	static Student TwinB;
	static Student holder;

	public static void main(String[] args) 
	{
		try
		{
			ArrayList<Student> dB = new ArrayList<Student>();
			String line; // string version of all student data
			File data = new File("all_students_jan.csv");///Users/gordie/Downloads/all_students_jan.csv
			FileReader fr = new FileReader(data);
			BufferedReader br = new BufferedReader(fr);
			br.readLine(); // eats junk title line at top of file
			File file = new File("filename.txt");//c/o http://www.mkyong.com/java/how-to-write-to-file-in-java-bufferedwriter-example/
			if (!file.exists()) {
				file.createNewFile();
			}
			FileWriter fw = new FileWriter(file.getAbsoluteFile());
			BufferedWriter bw = new BufferedWriter(fw);
			String print = "";

			//add info to database
			while ((line = br.readLine()) != null) {
				Student s =  new Student(line);
				dB.add(s);
			}
			//to print use System.out.println(dB.get(index));

			//pick your option
			String user = "";	//user
			Scanner input = new Scanner(System.in);	//user
			System.out.println("Type 1 to sort by last name. Type 2 to sort by twins. Type 3 to find students with the same birth month. Type 4 to sort by house" + 
					"\n" + "Type 5 to find the number of students of each gender. Type 6 to obtain the number of students who entered in 4th grade vs K" + "\n" + 
					"Type 7 to find the # of students born in 2001. Type 8 for first name frequency. Type 9 to sort Students by grade.");
			user = input.nextLine();

			if(user.equals("1")){
				System.out.println("type last name");
				user = input.nextLine();
				print = lastName(user, dB).toString();
				bw.write(print);
				bw.flush();
			}
			else if (user.equals("2")){
				print = twins(dB);
				bw.write(print);
				bw.flush();
			}
			else if (user.equals("3")){
				System.out.println("type the number of the month in MM format");
				user = input.nextLine();
				print = month(user, dB).toString();
				bw.write(print);
				bw.flush();
			}
			else if (user.equals("4")){
				System.out.println("type the House Advisor's last name. If more than 1 advisor pick one");
				user = input.nextLine();
				print = house(user, dB).toString();
				bw.write(print);
				bw.flush();
			}
			else if (user.equals("5")){
				print = gender(dB);
				bw.write(print);
				bw.flush();
			}
			else if (user.equals("6")){

				print = kv4(dB);
				bw.write(print);
				bw.flush();
			}
			else if (user.equals("7")){
				print = y2001(dB);
				bw.write(print);
				bw.flush();
			}
			else if (user.equals("8")){
				print = frequency(dB);
				bw.write(print);
				bw.flush();
			}
			else{
				System.out.println("select grade by entering the HS graduation year in YYYY format");
				user = input.nextLine();
				print = grade(user, dB).toString();
				bw.write(print);
				bw.flush();
			}


		}//try
		catch(Exception e)
		{
			System.out.println("file not found...dumbass");
		}
	}//main

	/**
	 * provides a list of students in a given house
	 * @param user		the given string
	 * @param dB		the complete database of students
	 * @return list
	 */
	private static Set<Student> house(String user, ArrayList<Student> dB) {
		Set<Student> list =  new HashSet<Student>();
		String HA = user.toString();
		for(int i = 0; i<dB.size(); i++){
			holder = new Student(dB.get(i).toString());
			if(holder.house.contains(HA)){
				list.add(holder);
			}
			else{
				continue;
			}
		}
		return list;
	}

	/**
	 * provides the number of students of each gender in the school
	 * @param dB		the complete database of students
	 * @return genders
	 */
	private static String gender(ArrayList<Student> dB) {
		String genders = "";
		int f = 0;
		int m = 0;
		for(int i = 0; i<dB.size(); i++){
			holder = new Student(dB.get(i).toString());
			if(holder.gender.equals("F")){
				f++;
			}
			else{
				m++;
			}
		}
		genders = "Male: " + m + " Female: " + f;
		return genders;
	}

	/**
	 * compares the number of people who entered in kindergarten to the number of people who entered in 4th grade
	 * @param dB		the complete database of students
	 * @return grade
	 */
	private static String kv4(ArrayList<Student> dB) {
		String grade = "";
		int k = 0;
		int v4 = 0;
		for(int i = 0; i<dB.size(); i++){
			holder = new Student(dB.get(i).toString());
			if(holder.grade_entered.equalsIgnoreCase("k")){
				k++;
			}
			else if (holder.grade_entered.equals(Integer.toString(4))){
				v4++;
			}
			else{
				continue;
			}
		}

		grade = "Kindergarten: " + k + " 4th grade: " + v4;
		return grade;
	}

	/**
	 * returns the number of students born in 2001
	 * @param dB		the complete database of students
	 * @return y2k
	 */
	private static String y2001(ArrayList<Student> dB) {
		String y2k = "";
		int y = 0;
		for(int i = 0; i<dB.size(); i++){
			holder = new Student(dB.get(i).toString());
			if(holder.year.equals("2001")){
				y++;
			}
			else{
				continue;
			}
		}

		y2k = "Number of students born in 2001: " + y;
		return y2k;
	}

	/**
	 * returns the top 10 first names
	 * @param dB		the complete database of students
	 * @return
	 */
	private static String frequency(ArrayList<Student> dB) {

		return null;
	}

	/**
	 * returns all of the students in a given grade
	 * @param user		the given string
	 * @param dB		the complete database of students
	 * @return list
	 */
	private static Set<Student> grade(String user, ArrayList<Student> dB) {
		Set<Student> list = new HashSet<Student>();
		String grad_Year = user.toString(); 
		for(int i = 0; i<dB.size(); i++){
			holder = new Student(dB.get(i).toString());
			if(holder.grad.equals(grad_Year)){
				list.add(holder);
			}
			else{
				continue;
			}
		}
	
		return list;
	}

	/**
	 * Finds all students born in a given month
	 * @param user		the given string
	 * @param dB		the complete database of students
	 * @return list
	 */
	private static Set<Student> month(String user, ArrayList<Student> dB) {
		Set<Student> list = new HashSet<Student>();
		String month_Num = user.toString(); 
		month_Num = month_Translator(month_Num);
		for(int i = 0; i<dB.size(); i++){
			holder = new Student(dB.get(i).toString());
			if(holder.month.equals(month_Num)){
				list.add(holder);
			}
			else{
				continue;
			}
		}
		return list;
	}

	private static String month_Translator(String month_Num) {
		int month_Digit = 0;
		if(month_Num.equals("01")){
			month_Digit = 1;
		}
		else if(month_Num.equals("02")){
			month_Digit = 2;
		}
		else if(month_Num.equals("03")){
			month_Digit = 3;
		}
		else if(month_Num.equals("04")){
			month_Digit = 4;
		}
		else if(month_Num.equals("05")){
			month_Digit = 5;
		}
		else if(month_Num.equals("06")){
			month_Digit = 6;
		}
		else if(month_Num.equals("07")){
			month_Digit = 7;
		}
		else if(month_Num.equals("08")){
			month_Digit = 8;
		}
		else if(month_Num.equals("09")){
			month_Digit = 9;
		}
		else if(month_Num.equals("10")){
			month_Digit = 10;
		}
		else if(month_Num.equals("11")){
			month_Digit = 11;
		}
		else{
			month_Digit = 12;
		}
		return Integer.toString(month_Digit);
	}

	/**
	 * Finds all students with a given last name
	 * @param user		the given string
	 * @param dB		the complete database of students
	 * @return last_Name
	 */
	private static Set<Student> lastName(String user, ArrayList<Student> dB) {
		Set<Student> last_Name = new HashSet<Student>();
		String last = user.toString();
		for(int i = 0; i<dB.size(); i++){
			holder = new Student(dB.get(i).toString());
			if(holder.last.equalsIgnoreCase(last)){
				last_Name.add(holder);
			}
			else{
				continue;
			}
		}


		return last_Name;
	}

	/**
	 * returns all twins
	 * @param dB		the complete database of students
	 * @return dSearch
	 */
	public static String twins (ArrayList<Student> dB ) {
		Set<Student> dSearch = new HashSet<Student>();
		for(int i=0; i<dB.size(); i++){
			TwinA = new Student(dB.get(i).toString());
			for(int j = i+1; j<dB.size(); j++){
				TwinB = new Student(dB.get(j).toString());
				if(TwinA.last.matches(TwinB.last) && TwinA.day.matches(TwinB.day) && TwinA.month.matches(TwinB.month) && TwinA.year.matches(TwinB.year)){
					//do Stuff
					//System.out.println("twins");
					dSearch.add(TwinA);
					dSearch.add(TwinB);
					break;
				}

				else{
					//do more nothing
					//System.out.println("no match");
					continue;
					//break;
				}

			}	
		}
		return dSearch.toString();
	}

}//class
