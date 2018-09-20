# Quiz-answer-check
CS II Homework


#include <iostream>
#include <string>
#include <iomanip>
#include <fstream>    // For file I/O

using namespace std;

void openInFile(ifstream& inFile, string fileName);
const float LOWEST_SCORE = 0.0f;

int main()
{
	ifstream ansFile, keyFile;
	ofstream outData;
	string name, answers;
	char ansChar, keyChar;
	int correct = 0, incorr = 0, noans = 0, possible =0;
	float score = 0.0f, percent = 0.0f;

	openInFile(ansFile, "hw1Ans.txt");
	openInFile(keyFile, "hw1Key.txt");
	outData.open("hw1Out.txt");

	outData << "Name         Correct   Incorr   No Ans   Total Score     %" << endl;
	outData << "----------   -------   ------   ------   -----------   -----" << endl;

	while (ansFile)
	{
		ansFile >> name;
		ansFile.get(ansChar);
		ansFile.get(ansChar);
		keyFile.get(keyChar);
		while (ansChar != '\n' && ansFile)
		{
			if (ansChar == '#')
			{
				noans++;
			}
			if (ansChar == keyChar)
			{
				correct++;
			}
			if(ansChar != keyChar && ansChar != '#')
			{ 
				incorr++;
			}
			possible++;
			ansFile.get(ansChar);
			keyFile.get(keyChar);
		}

		score = correct - (incorr * 0.25f);
		if (score < LOWEST_SCORE)
		{
			score = 0.0f;
		}

		percent = (score / possible) *100.00f;

		outData << fixed << setprecision(2) << name << setw(12) << correct << setw(12) << incorr << setw(12) << noans << setw(12) << score << setw(12) << percent << endl;
		
		correct = 0;
		incorr = 0;
		noans = 0;
		score = 0.0f;
		percent = 0.0f;
		possible = 0;
		
		keyFile.clear();
		keyFile.seekg(0);
	}



	return 0; 
}

void openInFile(ifstream& inText, string fileName)
{
	inText.open(fileName);
	if (!inText)
	{
		cout << "Can't open the input file.";
		exit(1);        // halts program                
	}
}
