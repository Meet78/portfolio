# RRS
Railway_Reservation_System

----------------------------------------------------------------------------------------------------------------

Queries

----------------------------------------------------------------------------------------------------------------

1) Fetch the data from the table by Name
     SELECT B.Train_Number, T.Train_Name
     FROM Booked B JOIN Train T on B.Train_Number = T.Train_Number
     WHERE B.Passenger_SSN IN (SELECT SSN FROM Passenger WHERE First_Name=? AND Last_Name=?)

2) Fetch the data from the table by Date
     SELECT P.First_Name, P.Last_Name, T.Train_Number, T.Train_Name, B.Ticket_Type
     FROM Train_Status TS
     FULL OUTER JOIN Train T ON T.Train_Name = TS.TrainName
     FULL OUTER JOIN Booked B ON B.Train_Number = T.Train_Number
     FULL OUTER JOIN Passenger P ON P.SSN = B.Passenger_SSN
     WHERE B.Status = 'Booked' AND TS.TrainDate = ?

4) Fetch the data from the table by Age
     SELECT Train.Train_Number, Train.Train_Name, Train.Source_Station, Train.Destination_Station,
     Passenger.First_Name, Passenger.Last_Name, Passenger.Address, Passenger.County,
     Booked.Ticket_Type, Booked.Status
     FROM Passenger
     FULL OUTER JOIN Booked ON Passenger.SSN = Booked.Passenger_SSN
     FULL OUTER JOIN Train ON Booked.Train_Number = Train.Train_Number
     WHERE substr(Passenger.Birth_Date,7,2) BETWEEN substr(STRFTIME('%Y',date('now', '-'||?||' YEAR')), 3, 2) AND
     substr(STRFTIME('%Y',date('now', '-'|| ? || ' YEAR')), 3, 2)

5) Fetch the data from the table by Ticket_Type
     SELECT Train.Train_Name, COUNT(*) as PassengerCount, Booked.Status
     FROM Booked
     FULL OUTER JOIN Train ON Booked.Train_Number = Train.Train_Number
     WHERE Booked.Ticket_Type = ?
     GROUP BY Train.Train_Name

6) Fetch the data from the table by Train_Name
     SELECT Passenger.First_Name, Passenger.Last_Name, Train.Train_Number, Train.Train_Name, Booked.Ticket_Type, Booked.Status
     FROM Passenger
     INNER JOIN Booked ON Passenger.SSN = Booked.Passenger_SSN
     INNER JOIN Train ON Booked.Train_Number = Train.Train_Number
     WHERE TRIM(Train.Train_Name) = ? AND Booked.Status = 'Booked'

7) Fetch the data from the table by Cancel ticket and waiting ticket
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


8) Waitlist Passanger Details

SELECT Passenger_SSN, Train_Number, Ticket_Type
    FROM Booked
    WHERE Status = 'WaitL'
    ORDER BY ROWID ASC
    LIMIT 1


9) Succefully booked ticket details

SELECT SSN, First_Name, Last_Name, Train.Train_Name, Booked.Status
        FROM Passenger
        INNER JOIN Booked ON Passenger.SSN = Booked.Passenger_SSN
        INNER JOIN Train ON Booked.Train_Number = Train.Train_Number
        WHERE Booked.Status = 'Booked'

----------------------------------------------------------------------------------------------------------------

Installation guide

----------------------------------------------------------------------------------------------------------------

To run this program you will have to install tkinter using pip command

- pip install tkinter

then run the program Code.py and input data and execute query.
