#include <iostream>
#include <fstream>
#include <sstream>
using namespace std;

//class session management

void createSession() {
    ofstream file("sessions.txt", ios::app);
    string code, date, time;
    int duration;

    cout << "\nCourse Code: ";
    cin >> code;
    cout << "Date (YYYY-MM-DD): ";
    cin >> date;
    cout << "Start Time (HH:MM): ";
    cin >> time;
    cout << "Duration (minutes): ";
    cin >> duration;

    file << code << "|" << date << "|" << time << "|" << duration << endl;
    file.close();

    cout << "Session created.\n";
}

void viewSessions() {
    ifstream file("sessions.txt");
    string line;

    cout << "\n--- Sessions ---\n";
    while (getline(file, line)) {
        cout << line << endl;
    }
    file.close();
}

//Taking of students attendance

void markAttendance() {
    ofstream file("attendance.txt", ios::app);

    string index, code, date, status;

    cout << "\nStudent Index: ";
    cin >> index;
    cout << "Course Code: ";
    cin >> code;
    cout << "Date: ";
    cin >> date;

    cout << "Status (Present/Absent/Late): ";
    cin >> status;

    file << index << "|" << code << "|" << date << "|" << status << endl;
    file.close();

    cout << "Attendance marked.\n";
}

void viewAttendance() {
    ifstream file("attendance.txt");
    string line;

    cout << "\n--- Attendance Records ---\n";
    while (getline(file, line)) {
        cout << line << endl;
    }
    file.close();
}

void updateAttendance() {
    ifstream file("attendance.txt");
    ofstream temp("temp.txt");

    string index, code, date;
    string line;

    cout << "\nEnter index to update: ";
    cin >> index;
    cout << "Course code: ";
    cin >> code;
    cout << "Date: ";
    cin >> date;

    bool found = false;

    while (getline(file, line)) {
        stringstream ss(line);
        string i,c,d,s;

        getline(ss,i,'|');
        getline(ss,c,'|');
        getline(ss,d,'|');
        getline(ss,s,'|');

        if (i==index && c==code && d==date) {
            cout << "New Status: ";
            cin >> s;
            temp << i << "|" << c << "|" << d << "|" << s << endl;
            found = true;
        } else {
            temp << line << endl;
        }
    }

    file.close();
    temp.close();

    remove("attendance.txt");
    rename("temp.txt","attendance.txt");

    if(found)
        cout << "Attendance updated.\n";
    else
        cout << "Record not found.\n";
}

// Main menu

void attendanceMenu() {
    int ch;
    do {
        cout << "1 Create Session\n";
        cout << "2 View Sessions\n";
        cout << "3 Mark Attendance\n";
        cout << "4 View Attendance\n";
        cout << "5 Update Attendance\n";
        cout << "6 Back\nChoice: ";
        cin >> ch;

        switch(ch) {
            case 1: createSession(); break;
            case 2: viewSessions(); break;
            case 3: markAttendance(); break;
            case 4: viewAttendance(); break;
            case 5: updateAttendance(); break;
        }

    } while(ch!=6);
}

// Sub menu

int main() {
    int choice;

    do {
        cout << "\n====== SYSTEM MENU ======\n";
        cout << "1 Student Management\n";
        cout << "2 Attendance Session Management\n";
        cout << "3 Exit\nChoice: ";
        cin >> choice;

        if(choice==1) attendanceMenu();

    } while(choice!=3);

    return 0;
}
