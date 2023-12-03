# RRS

Project Title -> Railway Reservation System

----------------------------------------------------------------------------------------------------------------

Queries

----------------------------------------------------------------------------------------------------------------

1) Fetch the data from the table by Name:

     SELECT B.Train_Number, T.Train_Name
     FROM Booked B JOIN Train T on B.Train_Number = T.Train_Number
     WHERE B.Passenger_SSN IN (SELECT SSN FROM Passenger WHERE First_Name=? AND Last_Name=?)

2) Fetch the data from the table by Date:

     SELECT P.First_Name, P.Last_Name, T.Train_Number, T.Train_Name, B.Ticket_Type
     FROM Train_Status TS
     FULL OUTER JOIN Train T ON T.Train_Name = TS.TrainName
     FULL OUTER JOIN Booked B ON B.Train_Number = T.Train_Number
     FULL OUTER JOIN Passenger P ON P.SSN = B.Passenger_SSN
     WHERE B.Status = 'Booked' AND TS.TrainDate = ?

3) Fetch the data from the table by Age:

     SELECT Train.Train_Number, Train.Train_Name, Train.Source_Station, Train.Destination_Station,
     Passenger.First_Name, Passenger.Last_Name, Passenger.Address, Passenger.County,
     Booked.Ticket_Type, Booked.Status
     FROM Passenger
     FULL OUTER JOIN Booked ON Passenger.SSN = Booked.Passenger_SSN
     FULL OUTER JOIN Train ON Booked.Train_Number = Train.Train_Number
     WHERE substr(Passenger.Birth_Date,7,2) BETWEEN substr(STRFTIME('%Y',date('now', '-'||?||' YEAR')), 3, 2) AND
     substr(STRFTIME('%Y',date('now', '-'|| ? || ' YEAR')), 3, 2)

4) Fetch the data from the table by Ticket_Type:

     SELECT Train.Train_Name, COUNT(*) as PassengerCount, Booked.Status
     FROM Booked
     FULL OUTER JOIN Train ON Booked.Train_Number = Train.Train_Number
     WHERE Booked.Ticket_Type = ?
     GROUP BY Train.Train_Name

5) Fetch the data from the table by Train_Name:
     SELECT Passenger.First_Name, Passenger.Last_Name, Train.Train_Number, Train.Train_Name, Booked.Ticket_Type, Booked.Status
     FROM Passenger
     INNER JOIN Booked ON Passenger.SSN = Booked.Passenger_SSN
     INNER JOIN Train ON Booked.Train_Number = Train.Train_Number
     WHERE TRIM(Train.Train_Name) = ? AND Booked.Status = 'Booked'

6) Fetch the data from the table by Cancel ticket and waiting ticket:

Create Trigger

CREATE TRIGGER IF NOT EXISTS update_waiting_list
    AFTER DELETE ON Booked
    BEGIN
        UPDATE Booked SET Status='Booked'
        WHERE Passenger_SSN IN (
            SELECT Passenger_SSN
            FROM Booked
            WHERE Train_Number = old.Train_Number AND Ticket_Type = old.Ticket_Type AND Status = 'WaitL'
            ORDER BY ROWID ASC
            LIMIT 1
        );
    END;


7) Waitlist Passanger Details:

SELECT Passenger_SSN, Train_Number, Ticket_Type
    FROM Booked
    WHERE Status = 'WaitL'
    ORDER BY ROWID ASC
    LIMIT 1


8) Succefully booked ticket details

SELECT SSN, First_Name, Last_Name, Train.Train_Name, Booked.Status
        FROM Passenger
        INNER JOIN Booked ON Passenger.SSN = Booked.Passenger_SSN
        INNER JOIN Train ON Booked.Train_Number = Train.Train_Number
        WHERE Booked.Status = 'Booked'

----------------------------------------------------------------------------------------------------------------

Installation guide

----------------------------------------------------------------------------------------------------------------

Python Installation:

Step 1 -> Download Python:

    Visit the official Python website at https://www.python.org/.
    Navigate to the "Downloads" section.
    Click on the "Latest Python x.x.x" button to download the installer.

Step 2 -> Run the Installer:

    Locate the downloaded installer file (it's usually a .exe file, e.g., python-3.x.x.exe).
    Double-click on the installer to run it.

Step 3 -> Installation Wizard:

    Check the box that says "Add Python x.x to PATH" during installation. This makes it easier to run Python from the command line.
    Click "Install Now" to start the installation.

Step 4 -> Verify Installation:

    Once the installation is complete, open the Command Prompt (or PowerShell).
    Type python --version or python -V and press Enter. This should display the installed Python version.


MySQL Installation:

Step 1 -> Download MySQL Installer:

    Visit the official MySQL website at https://dev.mysql.com/downloads/installer/.
    Click on the "MySQL Installer for Windows" download button.

Step 2 -> Run the Installer:

    Locate the downloaded installer file (usually a .msi file, e.g., mysql-installer-community-8.x.xx.msi).
    Double-click on the installer to run it.

Step 3 -> MySQL Installer Setup:

    Choose the "Developer Default" setup type for a standard installation that includes MySQL Server and other development tools.
    Click "Next" to proceed.

Step 4 -> Installation Process:

    The installer will download and install the selected components. This may take some time.

Step 5 -> Configuration:

    During installation, you'll be prompted to configure MySQL Server.
    Set a root password for the MySQL server. Make sure to remember this password.

Step 6 -> Complete the Installation:

    Once the installation is complete, click "Finish" to exit the installer.

Step 7 -> Verify Installation:

    Open the MySQL Command Line Client or MySQL Workbench (both installed by default).
    Enter the root password you set during installation.
    If you can log in without errors, the installation was successful.


To run this program you will have to install tkinter using pip command

- pip install tkinter

then run the program Code.py and input data and execute query.
