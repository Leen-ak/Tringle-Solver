# Triangle-Solver
/*
Triangle Solver (C++)
 Author:	Leen Aboukalil
 Date:      2024-02-03
 Purpose:	The purpose of this C++ console application is to assist engineers in the field by solving for the missing attributes of a triangle. This program provides a practical tool for calculating the length of the third side and the measures of the other two angles based on the given information. By automating these calculations, the application saves time and helps engineers accurately determine the complete characteristics of a triangle, aiding in various engineering and mathematical applications.
*/
#include <iostream>
#include <string>
#include <sstream>
#include <cmath>
#include <iomanip>
#include <cstring>
#include <numbers>
using namespace std;
using namespace std::numbers;

/*
fn:		convertToUpper
brief:	converting from lower case to upper case
param:	string
return: nothing (void)
*/
void convertToUpper(string& s)
{
	//convert the user input to upper case letter 
	for (char& a : s)
	{
		a = toupper(a);
	}

}

/*
fn:		RadToDeg
brief:	converting from radius to degree
param:	double
return: double
*/
double RadToDeg(double radius)
{
	return radius * (180 / pi);
}

/*
fn:		DegToRad
brief:	converting from degree to radius
param:	double
return: double
*/
double DegToRad(double degree)
{
	return degree * (pi / 180);
}

/*
fn:		calcPerimeter
brief:	calculate the perimeter
param:	double
return: double
*/
double calcPerimeter(double side1, double side2, double side3)
{
	return side1 + side2 + side3;
}

/*
fn:		calcArea
brief:	calculate the area for triangles
param:	double
return: double
*/
double calcArea(double side1, double side2, double side3)
{
	double semi_perimeter = (side1 + side2 + side3) / 2.0;
	return sqrt(semi_perimeter * (semi_perimeter - side1) * (semi_perimeter - side2) * (semi_perimeter - side3));

}

/*
fn:		myswap
brief:	to swap the values of the variables
param:	double
return: nothing (void)
*/
void myswap(double& a, double& b)
{
	double temp{};
	temp = a;
	a = b;
	b = temp;
}

/*
fn:		byside
brief:	to determine which side of the triangles
param:	double
return: nothing (void)
*/
void byside(double a, double b, double c)
{
	if (a == b && b == c && a == c)
	{
		cout << "Type      = " << "equilateral, ";
	}
	else if ((a == b && a != c) || (a == c && a != b) || (b == c && b != a))
	{
		cout << "Type      = " << "isosceles, ";
	}
	else if ((a != b && b != c && a != c))
	{
		cout << "Type      = " << "scalene, ";
	}
}

/*
fn:		by angles
brief:	to determine which type of angles the triangles
param:	double
return: nothing (void)
*/
void byangles(double alpha_deg, double beta_deg, double gamma_deg)
{
	if (alpha_deg < 90 && beta_deg < 90 && gamma_deg < 90)
		cout << "acute" << endl << endl;
	else if (alpha_deg == 90 || beta_deg == 90 || gamma_deg == 90)
		cout << "right" << endl << endl;
	else if (alpha_deg > 90 || beta_deg > 90 || gamma_deg > 90)
		cout << "obtuse" << endl << endl;
}

/*
fn:		by angles
brief:	to print the result of the program
param:	string, double
return: nothing (void)
*/
void print(string tringle_type, double a, double b, double c, double alpha, double beta, double gamma, double perimeter, double area)
{
	//Formatting the output of the first type of Triangles SSS
	cout << "\nFormat: " << tringle_type << endl;
	if (tringle_type == "SSA")
	{
		cout << "--> gamma\n";
	}
	cout << fixed << setprecision(3);
	cout << "a =" << "   " << a << "  " << "alpha =" << "   " << alpha << endl;
	cout << "b =" << "   " << b << "  " << "beta  =" << "   " << beta << endl;
	cout << "c =" << "   " << c << "  " << "gamma =" << "   " << gamma << endl;
	cout << "Perimeter =" << "   " << perimeter << endl;
	cout << "Area      =" << "   " << area << endl;
}


int main()
{
	//declare and initialize the variables 
	string tringle, tringle_type;
	string extra_input;
	double a{};
	double b{};
	double c{};
	double alpha_rad{}; double beta_rad{}; double gamma_rad{};
	double alpha_deg{}; double beta_deg{}; double gamma_deg{};
	double perimeter{}; double area{};
	double a_prime{}; double alpha_prime{}; double gamma_prime{};

	//output the program title 
	cout << "TriSolver 1.1.1 (c)2024 - 2, Leen Aboukhalil\n\n";

	//while loop continues running until the user types bye, exit, quite   
	while (true) {

		//take input from the user
		cout << "--> ";
		getline(cin, tringle);

		//calling convertToUpper method to convert the input to upper case 
		convertToUpper(tringle);

		//if statement to exit from the program  
		if (tringle == "BYE" || tringle == "QUIT" || tringle == "EXIT")
		{
			break;
		}

		//if there is any input in the stream store it in tringle_type variable 
		istringstream string_in(tringle);
		string_in >> tringle_type;
		convertToUpper(tringle_type);

		//Validation part for checking the 3 numbers in the stream and if there is any extra input 
		if (!(string_in >> a >> b >> c) || (string_in >> extra_input))
		{
			cout << "Bad command: Format # # #\n" << "where Format = SSS|SAS|ASA|SSA\n" << "      # = a real number\n" << endl;
			continue;
		}

		//validation for checking the string input not more or less than 3 strings 
		if (tringle_type.length() != 3)
		{
			cout << "Unknown command triangle format '" << tringle_type << endl;
			continue;
		}

		//if one of the input numbers is less than 0 multiplay by a negative number to get the positive numbers    
		{
			if (a < 0)
				a *= -1;
			else if (b < 0)
				b *= -1;
			else if (c < 0)
				c *= -1;
		}

		// Check the type of triangle and perform calculations accordingly
		if (tringle_type == "SSS")
		{
			//the calculation for SSS
			alpha_rad = acos((pow(b, 2) + pow(c, 2) - pow(a, 2)) / (2 * b * c));
			beta_rad = acos((pow(a, 2) + pow(c, 2) - pow(b, 2)) / (2 * a * c));
			gamma_rad = pi - alpha_rad - beta_rad;

			//calling the method to convert from radians to degree
			alpha_deg = RadToDeg(alpha_rad);
			beta_deg = RadToDeg(beta_rad);
			gamma_deg = RadToDeg(gamma_rad);

			//calling the method to calculate the Perimeter and Area 
			perimeter = calcPerimeter(a, b, c);
			area = calcArea(a, b, c);

			//the condition for an impossible triangle
			if (((a + b <= c) || (a + c <= b) || (b + c <= a)) && alpha_deg + beta_deg + gamma_deg != 180)
			{
				cout << "Impossible triangle\n";
			}
			else
			{
				//callint the print method to print the output 
				print(tringle_type, a, b, c, alpha_deg, beta_deg, gamma_deg, perimeter, area);
				byside(a, b, c);
				byangles(alpha_deg, beta_deg, gamma_deg);
			}
		}
		else if (tringle_type == "SAS")
		{
			//the calculation for SAS
			gamma_rad = DegToRad(b);
			myswap(b, c);
			c = sqrt(pow(a, 2) + pow(b, 2) - 2 * a * b * cos(gamma_rad));
			alpha_rad = acos((pow(b, 2) + pow(c, 2) - pow(a, 2)) / (2.0 * b * c));
			beta_rad = pi - alpha_rad - gamma_rad;

			//calling the method to convert from radians to degree
			alpha_deg = RadToDeg(alpha_rad);
			beta_deg = RadToDeg(beta_rad);
			gamma_deg = RadToDeg(gamma_rad);

			//calling the method to calculate the Perimeter and Area 
			perimeter = calcPerimeter(a, b, c);
			area = calcArea(a, b, c);

			//calling the print method to print the output 
			print(tringle_type, a, b, c, alpha_deg, beta_deg, gamma_deg, perimeter, area);
			byside(a, b, c);
			byangles(alpha_deg, beta_deg, gamma_deg);
		}
		else if (tringle_type == "ASA")
		{
			//calling the method to convert from degree to radians
			alpha_rad = DegToRad(a);
			gamma_rad = DegToRad(c);

			//the calculation for ASA
			beta_rad = pi - alpha_rad - gamma_rad;
			a = b * (sin(alpha_rad) / sin(beta_rad));
			c = b * (sin(gamma_rad) / sin(beta_rad));

			//calling swap method to swap the values 
			swap(beta_rad, gamma_rad);

			//calling the method to convert from radians to degree
			alpha_deg = RadToDeg(alpha_rad);
			beta_deg = RadToDeg(beta_rad);
			gamma_deg = RadToDeg(gamma_rad);

			//calling the method to calculate the Perimeter and Area 
			perimeter = calcPerimeter(a, b, c);
			area = calcArea(a, b, c);

			//calling swap method to swap the values 
			myswap(b, c);

			//calling the print method to print the output
			print(tringle_type, a, b, c, alpha_deg, beta_deg, gamma_deg, perimeter, area);
			byside(a, b, c);
			byangles(alpha_deg, beta_deg, gamma_deg);

		}
		else if (tringle_type == "SSA")
		{
			//the calculation for SSA
			beta_rad = DegToRad(c);
			gamma_rad = asin((b / a) * sin(beta_rad));
			alpha_rad = pi - beta_rad - gamma_rad;
			c = a * (sin(alpha_rad) / sin(beta_rad));

			//calling the method to convert from radians to degree
			alpha_deg = RadToDeg(alpha_rad);
			beta_deg = RadToDeg(beta_rad);
			gamma_deg = RadToDeg(gamma_rad);

			//calling swap method to swap the values 
			myswap(a, c);
			myswap(c, b);

			//calling the method to calculate the Perimeter and Area 
			perimeter = calcPerimeter(a, b, c);
			area = calcArea(a, b, c);

			//calling the print method to print the output
			print(tringle_type, a, b, c, alpha_deg, beta_deg, gamma_deg, perimeter, area);
			byside(a, b, c);
			byangles(alpha_deg, beta_deg, gamma_deg);

			//calling the method to convert from degree to radians
			alpha_deg = DegToRad(alpha_rad);
			beta_deg = DegToRad(beta_rad);
			gamma_deg = DegToRad(gamma_rad);

			//the calculation for (the equation's right side)
			double D = (c / b) * sin(beta_rad);

			//there are four possible cases for SSA
			if (D > 1)
			{
				cout << "Impossible triangle" << endl;
			}
			else if (D == 1)
			{
				gamma_rad = 90;
				print(tringle_type, a, b, c, alpha_deg, beta_deg, gamma_rad, perimeter, area);
			}
			else if (D < 1)
			{
				if (b >= c && beta_rad >= gamma_rad)
				{
					gamma_rad = asin(D);
					gamma_deg = RadToDeg(gamma_rad);
					print(tringle_type, a, b, c, alpha_deg, beta_deg, gamma_deg, perimeter, area);
				}
				else if (b < a)
				{
					//the calculation for the second triangle   
					gamma_rad = asin(D);
					gamma_prime = pi - gamma_rad;
					alpha_rad = pi - beta_rad - gamma_rad;
					alpha_prime = pi - beta_rad - gamma_prime;
					a_prime = b * (sin(alpha_prime) / sin(beta_rad));

					//calling the method to convert from radians to degree
					alpha_prime = RadToDeg(alpha_prime);
					gamma_prime = RadToDeg(gamma_prime);
					beta_deg = RadToDeg(beta_rad);

					//calling the method to calculate the Perimeter and Area 
					perimeter = calcPerimeter(a_prime, b, c);
					area = calcArea(a_prime, b, c);

					//calling the print method to print the output
					cout << "--> gamma`\n";
					cout << fixed << setprecision(3);
					cout << "a =" << "   " << a_prime << "  " << "alpha =" << "   " << alpha_prime << endl;
					cout << "b =" << "   " << b << "  " << "beta  =" << "   " << beta_deg << endl;
					cout << "c =" << "   " << c << "  " << "gamma =" << "   " << gamma_prime << endl;
					cout << "Perimeter =" << "   " << perimeter << endl;
					cout << "Area      =" << "   " << area << endl;
					byide(a_prime, b, c);
					byangles(alpha_prime, beta_deg, gamma_prime);

				}
			}
		}
	}
}

