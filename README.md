///   Paulson Jaimy   ///

//  Random Walks //

#include<iostream>
#include<vector>
#include<cmath>
#include<algorithm>
#include<iomanip>
using namespace std;
#ifndef M_PI
#define M_PI (3.14159265358979323846)
#endif


class Walk {
public:
	Walk() : x(0), y(0), distance(0) {}

	double getX() const { return x; }
	double getY() const { return y; }
	double getDistance() const { return distance; }

	void setX(double newX) {
		x = newX; 
	}
	void setY(double newY) {
		y = newY;
	}
	void setDistance(double newDistance) {
		distance = newDistance; 
	}

	int generateAngle() {
		return rand() % 359 + 1;
	}

	int generateLength() {
		return rand() % 100 + 1;
	}

	double convertToRadians(double degree) {

		double radians = (degree * M_PI) / 180.0;
		return radians;
	}

	void walkAgain() {
		int angle = generateAngle();
		int length = generateLength();
		double radians = convertToRadians(angle);
		double changex;
		double changey;

		changex = length * cos(radians);
		changey = length * sin(radians);

		x += changex;
		y += changey;
		distance += length;

	}

	friend istream& operator >> (istream& input, Walk& w) {
		input >> w.x >> w.y;
		return input;
		}

	    friend ostream& operator <<(ostream & output , const Walk& w) {
		output << "Point: " << fixed <<setprecision(2) << w.getX() << ", " << w.getY() << " Distance: " << w.getDistance();
		return output;
	}



private:
	double x;
	double y;
	double distance;
};

int main() {
	int numSegments;
	int numWalks;
	vector<Walk>walks;

	cout << "*** Random Walks ***" << endl<< endl;
	cout << "This program genrates a number of random walks." << endl << endl;
	cout << "Enter number of points for each walk: ";
	cin >> numSegments;
	cout << endl << endl;
	cout << "Enter number of random walks to generate: ";
	cin >> numWalks;
	cout << endl << endl;

	

	for (int i = 0; i < numWalks; ++i) {
		Walk walk;
		for (int j = 0; j < numSegments; ++j) {
			walk.walkAgain();

			cout << "Walk/Step:" << i + 1 << "/" << j + 1 << ": " << walk << endl;
		}

		walks.push_back(walk);

		cout << endl << endl;
	}

	double totalDistance = 0;
	vector<double> distance;

	for (const Walk& walk : walks) {
		totalDistance += walk.getDistance();
		distance.push_back(walk.getDistance());
	}
	sort(distance.begin(), distance.end());

	double averageDistance = totalDistance / numWalks;
	double shortestDistance = *min_element(distance.begin(), distance.end());

	
	double medianDistance = (numWalks % 2 == 0) ? (distance [numWalks / 2 - 1] + distance[numWalks / 2]) / 2 : distance[numWalks / 2];
	double longestDistance = *max_element(distance.begin(), distance.end());

	cout << "Walks Sorted:" << endl;
	for (const Walk& walk : walks) {
		cout << " " << walk << endl;

	}
	cout << endl;

	cout << "Average Distance: "  << averageDistance << endl<<endl;
	cout << "Shortest Distance/Walk: " << shortestDistance << endl;
	cout << "Median Distance/Walk: " << medianDistance << endl;
	cout << "Longest Distance/Walk: " << longestDistance << endl;

	return 0;

}
