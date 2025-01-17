STEPS FOR CLONING OUR PROJECT FROM GITHUB:
1. Install Git (if you don’t have it already)
•	Git is a tool that lets you download and manage code from websites like GitHub.
•	If you don’t have Git installed, download and install it from here.
2. Get the Project’s Link
•	Go to the website where the project is hosted (like GitHub).
•	Find the "Clone" or "Code" button on the project page.
•	Click it and copy the link (it will either look like https://github.com/username/project.git or git@github.com:username/project.git).
3. Open the Terminal or Command Prompt
•	On your computer, open the Terminal (Mac/Linux) or Git Bash (Windows).
•	It’s like the app where you type commands.
4. Go to the Folder Where You Want the Project
Decide where on your computer you want to save the project. For example, if you want to put it in a folder called “projects”:
TYPE:
cd ~/projects
(You can skip this step if you’re happy with where you are.)
5. Download the Project
Now, to download (or “clone”) the project, type the command:
git clone https://github.com/username/project.git

or
git clone git@github.com:username/project.git


6. Enter the Project Folder
After cloning, you’ll have a copy of the project in your folder. To open the project, type:
cd project

INSTALLING POSTRESQL :

1. Visit the Official PostgreSQL Download Page:
Go to the PostgreSQL official download page for all operating systems:
https://www.postgresql.org/download/

2. For macOS:
1.	Visit the macOS download page:
PostgreSQL macOS Download
2.	You will be redirected to the EnterpriseDB site. Click on "Download the installer".
3.	Select the version you want and download the .dmg file.
4.	Once downloaded, open the .dmg file and follow the on-screen instructions to install PostgreSQL.
5.	pgAdmin (PostgreSQL’s graphical management tool) will also be installed. You can use it to manage your PostgreSQL databases.

3. For Windows:
1.	Visit the Windows download page:
PostgreSQL Windows Download
2.	You will be redirected to the EnterpriseDB site. Click on "Download the installer".
3.	Select your version of PostgreSQL and download the .exe file.
4.	Run the installer. Follow the installation wizard steps (you can choose the default settings or customize as needed).
5.	After installation, you can use pgAdmin or the Command Prompt to interact with PostgreSQL.





4. Verify Installation (For All OS)
Once PostgreSQL is installed, you can verify that it is working by checking the version:
TYPE CODE:
psql --version
To connect to PostgreSQL, use:
TYPE CODE:
psql -U postgres





CREATION OF DB AND RELATIONS:


1.	CREATE DATABASE:
	
   create database C3;

2. CREATE ALL TABLES:

	-- Students Table
CREATE TABLE Students ( 
    ROLL_NO VARCHAR(50) PRIMARY KEY, 
    STUDENT_NAME VARCHAR(255) NOT NULL,
    password varchar (255) not null, 
    DEPARTMENT VARCHAR(255), 
    CONTACT_NO VARCHAR(15) 
); 

	-- Events Table
CREATE TABLE Events ( 
    EVENT_ID SERIAL PRIMARY KEY, 
	club_id int ,
    EVENT_NAME VARCHAR(255) NOT NULL, 
    EVENT_DATE DATE, 
    EVENT_VENUE VARCHAR(255), 
    EVENT_TIME TIMESTAMP, 
    EVENT_POSTER_IMAGE TEXT, 
    EVENT_SHORT_DESCRIPTION TEXT, 
    EVENT_LINK TEXT, 

 EXTRA_CURRICULAR_OR_NON_CURRICULAR TEXT 
	foreign key (club_id) references clubs(club_id) on delete cascade
);
	-- Clubs Table
CREATE TABLE Clubs ( 
    CLUB_ID SERIAL PRIMARY KEY, 
    CLUB_NAME VARCHAR(50) NOT NULL, 
    CLUB_DESCRIPTION VARCHAR(500), 
    CLUB_LOGO TEXT 
); 

	-- Office Bearers Table
CREATE TABLE OfficeBearers (
	ob_id serial primary key, 
    CLUB_ID INT NOT NULL, 
    OB_CONTACT_DETAILS VARCHAR(50) NOT NULL, 
    OB_PASSWORD VARCHAR(255) NOT NULL, 
    OB_POST VARCHAR(50), 
    OB_IMAGE TEXT, 
    STUDENT_ROLL_NO VARCHAR(50),  
    FOREIGN KEY (CLUB_ID) REFERENCES Clubs(CLUB_ID) ON DELETE CASCADE, 
    FOREIGN KEY (STUDENT_ROLL_NO) REFERENCES Students(ROLL_NO) ON DELETE CASCADE 
); 

	-- Member Table
CREATE TABLE Member ( 
member_id serial primary key,
    CLUB_ID INT NOT NULL, 
    STUDENT_ROLL_NO VARCHAR(50), 
    MEM_PASSWORD VARCHAR(255) NOT NULL, 
    FOREIGN KEY (CLUB_ID) REFERENCES Clubs(CLUB_ID) ON DELETE CASCADE, 
    FOREIGN KEY (STUDENT_ROLL_NO) REFERENCES Students(ROLL_NO) ON DELETE CASCADE 
); 

	-- Academic Table
CREATE TABLE Academic (
	academic_id serial primary key, 
    STUDENT_ROLL_NO VARCHAR(50), 
    SEM INT, 
    GPA DECIMAL(4, 2), 
    FOREIGN KEY (STUDENT_ROLL_NO) REFERENCES Students(ROLL_NO) ON DELETE CASCADE 
); 

	-- IntraAchievements Table
CREATE TABLE IntraAchievements ( 
    ACHIEVEMENT_ID SERIAL primary key, 
    EVENT_ID INT, 
    ROLL_NO VARCHAR(50) NOT NULL, 
    ACHIEVEMENTS_DESC TEXT, 
    PLACE_WON TEXT, 
    CERTIFICATE_LINK TEXT, 
    FOREIGN KEY (ROLL_NO) REFERENCES Students(ROLL_NO) ON DELETE CASCADE, 
    FOREIGN KEY (EVENT_ID) REFERENCES Events(EVENT_ID) ON DELETE CASCADE 
); 

	-- InterAchievements Table
CREATE TABLE InterAchievements ( 
    INTER_ID SERIAL primary key, 
    ROLL_NO VARCHAR(50) NOT NULL, 
    CONDUCTING_ORGANIZATION TEXT, 
    INTER_EVENT_NAME TEXT, 
    INTER_ACHIEVEMENTS_DESC TEXT, 
    PLACE_WON TEXT, 
    CERTIFICATE_LINK TEXT, 
    FOREIGN KEY (ROLL_NO) REFERENCES Students(ROLL_NO) ON DELETE CASCADE 
); 

 

	-- Participation Table
CREATE TABLE Participation ( 
    PARTICIPATION_ID SERIAL primary key, 
    ROLL_NO VARCHAR(50) NOT NULL, 
    EVENT_ID INT,  
    FOREIGN KEY (ROLL_NO) REFERENCES Students(ROLL_NO) ON DELETE CASCADE, 
    FOREIGN KEY (EVENT_ID) REFERENCES Events(EVENT_ID) ON DELETE CASCADE 
); 

	-- Reply Table

CREATE TABLE Reply(
	FEEDBACK_ID serial primary key,
	REPLY TEXT,
	FEEDBACK_TEXT VARCHAR(255),
	ROLL_NO VARCHAR(255),
	CLUB_ID BIGINT,
	FOREIGN KEY (ROLL_NO) REFERENCES Students(ROLL_NO) ON DELETE CASCADE, 
	FOREIGN KEY (CLUB_ID) REFERENCES Clubs(CLUB_ID) ON DELETE CASCADE,
);

	-- Temp_replies Table
CREATE TABLE Temp_replies(
	FEEDBACK_ID INT primary key,
	REPLY_TEXT TEXT
);

	- -  FEEDBACK Table
CREATE TABLE FEEDBACK(
	FEEDBACK_ID serial primary key,
	ROLL_NO VARCHAR(255),
	EVENT_ID INT,
	CLUB_ID INT,
	GRIEVANCE TEXT,
	FOREIGN KEY (ROLL_NO) REFERENCES Students(ROLL_NO) ON DELETE CASCADE, 
	FOREIGN KEY (CLUB_ID) REFERENCES Clubs(CLUB_ID) ON DELETE CASCADE,
	    FOREIGN KEY (EVENT_ID) REFERENCES Events(EVENT_ID) ON DELETE CASCADE 
); 

	-- ANNOUNCEMENTS Table
CREATE TABLE ANNOUNCEMENTS(
	ANNOUNCEMENT_ID serial primary key,
	EVENT_ID INT,
	ANNOUNCEMENT_TYPE varchar(255),
	ANNOUNCEMENT_DESCRIPTION TEXT,
	CLUB_ID INT,
	FOREIGN KEY (CLUB_ID) REFERENCES Clubs(CLUB_ID) ON DELETE CASCADE,
	    FOREIGN KEY (EVENT_ID) REFERENCES Events(EVENT_ID) ON DELETE CASCADE 
);


3. CREATE  TRIGGERS:
	

CREATE OR REPLACE FUNCTION after_feedback_delete_trigger_func()
RETURNS TRIGGER AS $$
DECLARE
    ob_reply TEXT;
BEGIN
    -- Get the reply text from the temporary table
    SELECT reply_text INTO ob_reply
    FROM temp_replies
    WHERE feedback_id = OLD.feedback_id;

    -- If no reply text is found, use a default message
    IF ob_reply IS NULL THEN
        ob_reply := 'No reply provided';
    END IF;

    -- Insert into reply with distinct reply and feedback text
    INSERT INTO reply (
        feedback_id, 
        reply,        -- OB's reply
        roll_no,
        feedback_text, -- Original feedback from non-member
        club_id
    )
    VALUES (
        OLD.feedback_id, 
        ob_reply,     -- OB's reply text
        OLD.roll_no,
        OLD.grievance, -- Original feedback
        OLD.club_id
    );

    -- Clean up the temporary storage
    DELETE FROM temp_replies WHERE feedback_id = OLD.feedback_id;

    RETURN NULL;
END;
$$ LANGUAGE plpgsql;




CREATE TRIGGER after_feedback_delete
AFTER DELETE ON feedback
FOR EACH ROW
EXECUTE FUNCTION after_feedback_delete_trigger_func();

5.GET THE API KEY FOR RECAPTCHA VERIFICATION FROM THE BELOW WEBSITE :
https://developers.google.com/recaptcha/intro

5.CREATE A .env FILE and 

SESSION_SECRET="your_secret"
PG_USER="your_username"
PG_HOST="localhost"
PG_DATABASE="your_database_name"
PG_PASSWORD="your_password"
PG_PORT="5432"
RECAPTCHA_SITE_KEY=your_site_key_without_qoutes
RECAPTCHA_SECRET_KEY=your_secret_key_without_quotes
PORT="3000"

6.REPLACE LINE 771 AND 830 IN ROUTES.JS (    const targetDir = path.join('D:\\WebDev Projects\\CEG-Club-Connex\\public\\images\\uploads');
WITH paths relative to your PC.





RUN ON VSCODE:

1.	Open the Terminal in VS Code (if not already opened).
2.	If the project uses a package manager like npm (for Node.js projects), pip (for Python projects), or composer (for PHP projects), you will need to install the project dependencies
In the terminal, run:
npm instalL


3. For Node.js/JavaScript Projects: If the project has a start script or server file, run:
    node index.js

4. Open your browser and go to the provided URL (e.g., http://localhost:3000 for a React project).








