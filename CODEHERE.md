#include <iostream>
#include <string>
#include <vector>  // For the command handling function.
#include <cctype>  // Will be used to eliminate case sensitivity problems.
#include <stdlib.h>

using namespace std;

enum en_DIRS { NORTH, EAST, SOUTH, WEST };
enum en_ROOMS { E, D, A, B, F, C, G, H, I, J, K };

const int NONE = -1;
const int DIRS = 4;
const int ROOMS = 11;

struct word
{
	string words_;
	int code;
};

struct room
{
	string description;
	int roomExit[DIRS];
};

// -------------------------------------------------------------------------------------------------

void setLocation(room *rooms_)
{
	rooms_[E].description.assign("E");
	rooms_[E].roomExit[NORTH] = NONE;
	rooms_[E].roomExit[EAST] = NONE;
	rooms_[E].roomExit[SOUTH] = A;
	rooms_[E].roomExit[WEST] = NONE;

	rooms_[D].description.assign("D");
	rooms_[D].roomExit[NORTH] = NONE;
	rooms_[D].roomExit[EAST] = NONE;
	rooms_[D].roomExit[SOUTH] = B;
	rooms_[D].roomExit[WEST] = NONE;

	rooms_[A].description.assign("A");
	rooms_[A].roomExit[NORTH] = E;
	rooms_[A].roomExit[EAST] = B;
	rooms_[A].roomExit[SOUTH] = NONE;
	rooms_[A].roomExit[WEST] = NONE;

	rooms_[B].description.assign("B");
	rooms_[B].roomExit[NORTH] = D;
	rooms_[B].roomExit[EAST] = F;
	rooms_[B].roomExit[SOUTH] = C;
	rooms_[B].roomExit[WEST] = A;

	rooms_[F].description.assign("F");
	rooms_[F].roomExit[NORTH] = NONE;
	rooms_[F].roomExit[EAST] = NONE;
	rooms_[F].roomExit[SOUTH] = NONE;
	rooms_[F].roomExit[WEST] = B;

	rooms_[C].description.assign("C");
	rooms_[C].roomExit[NORTH] = B;
	rooms_[C].roomExit[EAST] = G;
	rooms_[C].roomExit[SOUTH] = I;
	rooms_[C].roomExit[WEST] = NONE;

	rooms_[G].description.assign("G");
	rooms_[G].roomExit[NORTH] = NONE;
	rooms_[G].roomExit[EAST] = NONE;
	rooms_[G].roomExit[SOUTH] = NONE;
	rooms_[G].roomExit[WEST] = C;

	rooms_[H].description.assign("H");
	rooms_[H].roomExit[NORTH] = NONE;
	rooms_[H].roomExit[EAST] = I;
	rooms_[H].roomExit[SOUTH] = K;
	rooms_[H].roomExit[WEST] = NONE;

	rooms_[I].description.assign("I");
	rooms_[I].roomExit[NORTH] = C;
	rooms_[I].roomExit[EAST] = J;
	rooms_[I].roomExit[SOUTH] = NONE;
	rooms_[I].roomExit[WEST] = H;

	rooms_[J].description.assign("J");
	rooms_[J].roomExit[NORTH] = NONE;
	rooms_[J].roomExit[EAST] = NONE;
	rooms_[J].roomExit[SOUTH] = NONE;
	rooms_[J].roomExit[WEST] = I;

	rooms_[K].description.assign("K");
	rooms_[K].roomExit[NORTH] = H;
	rooms_[K].roomExit[EAST] = NONE;
	rooms_[K].roomExit[SOUTH] = NONE;
	rooms_[K].roomExit[WEST] = NONE;
}

// -------------------------------------------------------------------------------------------------

void set_directions(word *dir)
{
	dir[NORTH].code = NORTH;
	dir[NORTH].words_ = "NORTH";
	dir[EAST].code = EAST;
	dir[EAST].words_ = "EAST";
	dir[SOUTH].code = SOUTH;
	dir[SOUTH].words_ = "SOUTH";
	dir[WEST].code = WEST;
	dir[WEST].words_ = "WEST";
}

// -------------------------------------------------------------------------------------------------

void section_command(string Cmd, string &wd1, string &wd2)
{
	string sub_str;
	vector<string> words;
	char search = ' ';
	size_t i, j;

	// Split Command into vector
	for (i = 0; i < Cmd.size(); i++)
	{
		if (Cmd.at(i) != search)
		{
			sub_str.insert(sub_str.end(), Cmd.at(i));
		}
		if (i == Cmd.size() - 1)
		{
			words.push_back(sub_str);
			sub_str.clear();
		}
		if (Cmd.at(i) == search)
		{
			words.push_back(sub_str);
			sub_str.clear();
		}
	}
	// Clear out any blanks
	// I work backwards through the vectors here as a cheat not to invaldate the iterator
	for (i = words.size() - 1; i > 0; i--)
	{
		if (words.at(i) == "")
		{
			words.erase(words.begin() + i);
		}
	}
	// Make words upper case
	// Right here is where the functions from cctype are used
	for (i = 0; i < words.size(); i++)
	{
		for (j = 0; j < words.at(i).size(); j++)
		{
			if (islower(words.at(i).at(j)))
			{
				words.at(i).at(j) = toupper(words.at(i).at(j));
			}
		}
	}
	// Very simple. For the moment I only want the first to words at most (verb / noun).
	if (words.size() == 0)
	{
		cout << "No command given" << endl;
	}
	if (words.size() == 1)
	{
		wd1 = words.at(0);
	}
	if (words.size() == 2)
	{
		wd1 = words.at(0);
		wd2 = words.at(1);
	}
	if (words.size() > 2)
	{
		cout << "Command too long. Only type one or two words (direction or verb and noun)" << endl;
	}
}

// ----------------------------------------------------------------------------------------

bool parser(int &loc, string wd1, string wd2, word *dir, room *rms)
{
	int i;
	for (i = 0; i < DIRS; i++)
	{
		if (wd1 == dir[i]. words_)
		{
			if (rms[loc].roomExit[dir[i].code] != NONE)
			{
				loc = rms[loc].roomExit[dir[i].code];
				cout << "You are now in a  " << rms[loc].description << "." << endl;
				if (loc == K)
				{
					cout << "You have made it to the end of the maze, congratulations!" << endl;
					system("PAUSE");
					exit(0);

				}
				else
				return true;
			}
			else
			{
				cout << "No exit that way." << endl;
				return true;
			}
		}
	}
	cout << "Not a valid direction. Enter, north, east, south or west." << endl;
	return false;
}

// ----------------------------------------------------------------------------------------

int main()
{
	string command;
	string word_1;
	string word_2;

	room rooms[ROOMS];
	setLocation(rooms);

	word directions[DIRS];
	set_directions(directions);

	int location = A; // using the enumerated type identifier, of course.
	int endLocation = K;

	while (word_1 != "QUIT")
	{
		command.clear();
		cout << "What shall I do? ";
		getline(cin, command);
		//cout << "Your raw command was " << command << endl;

		word_1.clear();
		word_2.clear();

		// Call the function that handles the command line format.
		section_command(command, word_1, word_2);

		// Call the parser.
		parser(location, word_1, word_2, directions, rooms);

	}
	return 0;
}
