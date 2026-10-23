# 📅 AI Timetable Reminder Agent

**Automated College Timetable Reminders Using Make.com, Google Sheets, and Gmail**

## 📌 Overview

The AI Timetable Reminder Agent is a workflow automation project that retrieves a student's college timetable, picks out the classes scheduled for today, formats them neatly, and delivers them straight to your inbox through Gmail.

It is my **first automation project**, built through plenty of trial, error, and debugging, and it shows how no-code tools can solve an everyday problem by connecting different apps into one smooth workflow.

> **Note:** The current implementation uses predefined rules and workflow automation. It does not currently use a Large Language Model (LLM). An AI-powered version is part of the future scope.

## ❗ Problem Statement

Every morning, students have to open their timetable and figure out which subjects, class timings, and practical sessions are scheduled for the day. With several classes in a day, doing this by hand gets tedious and is easy to forget.

The key problems are:
- Manually checking the timetable every morning.
- Finding the classes scheduled for the current day.
- Organizing subject names and timings into a readable format.
- Remembering to check the timetable before leaving for college.

## 💡 Solution

This project automates the entire daily reminder process using Make.com.

The workflow reads timetable data from Google Sheets, identifies the classes that match the current weekday, combines them into one cleanly formatted message, and sends it through Gmail, with no manual checking required.

### Key Features

- **Automated data retrieval:** Reads timetable records directly from Google Sheets.
- **Day-based filtering:** Selects only the classes that match the current weekday.
- **Data aggregation:** Combines multiple class records into a single email.
- **Formatted email:** Shows the subject first, followed by the start and end times.
- **Scheduled execution:** Runs automatically once the scenario is activated and scheduled.
- **Weekday restriction:** Includes a separate filter intended to prevent weekend emails.

## 🛠️ Technologies Used

| Technology | Purpose |
|---|---|
| Make.com | Workflow automation and module integration |
| Google Sheets | Stores the college timetable |
| Gmail | Sends daily timetable reminders |
| Filters | Controls which timetable records and emails proceed |
| Text Aggregator | Combines class records into a single message |
| HTML and CSS | Formats the email content and preserves line breaks |

## 🔄 Workflow Architecture

The automation runs through five main stages:

1. **Google Sheets — Search Rows**
   - Retrieves the timetable records.
   - Reads the Day, Start Time, End Time, and Subject columns.

2. **Today's Classes Only — Filter**
   - Compares the current weekday with the weekday stored in each timetable row.
   - Lets only the matching records continue.

3. **Tools — Text Aggregator**
   - Merges the matching class records into one message.
   - Formats each class with its subject and timings.
   - Separates different classes with a blank line.

4. **Weekdays Only — Filter**
   - Checks whether the email should be sent on the current day.
   - Blocks the email when the condition is not satisfied.

5. **Gmail — Send an Email**
   - Receives the aggregated timetable.
   - Uses HTML formatting to preserve the intended layout.
   - Sends the reminder to the configured email address.

### Workflow Diagram

Google Sheets  
↓  
Today's Classes Only Filter  
↓  
Text Aggregator  
↓  
Weekdays Only Filter  
↓  
Gmail — Send Email

## ⚙️ Filter Logic

### 1. Today's Classes Only

**Purpose:** Select only the timetable records that belong to the current weekday.

**Logic:**

- Get the current weekday using Make.com's `formatDate()` function.
- Compare the result with the mapped `Day` value from Google Sheets.
- Continue only when both values match.

Conceptually:

`Current Weekday = Timetable Row Day`

For example, on Monday the workflow selects only the rows where the Day column contains `Monday`.

### 2. Weekdays Only

**Purpose:** Prevent the timetable reminder from being sent on Saturday and Sunday.

The filter checks whether the current day falls in the Monday–Friday range. If it does not, the email is blocked.

> **Tip:** Test the weekend behavior explicitly before relying on the automation.

## 🔍 How It Works

### Step 1: Store the Timetable

The timetable lives in Google Sheets with four columns:

- `Day`
- `Start Time`
- `End Time`
- `Subject`

### Step 2: Retrieve Timetable Records

Make.com's Google Sheets module searches the sheet and returns all available rows.

### Step 3: Identify Today's Classes

The first filter compares the current weekday with each row's Day value. Only matching classes move forward.

### Step 4: Combine the Classes

The Text Aggregator turns multiple matching records into one formatted message.

Example:

Subject: OS  
Start: 9:20 am  
End: 10:15 am

Subject: OOPJ  
Start: 10:45 am  
End: 11:40 am

### Step 5: Check the Weekday Restriction

The second filter decides whether the email is allowed to proceed on the current day.

### Step 6: Format and Send the Email

Gmail receives the aggregated output. The HTML email body preserves line breaks and keeps each class clearly separated for easy reading.

### Step 7: Run Automatically

Once the scenario is activated and the schedule is set, Make.com runs the workflow at the chosen time with no need for a manual "Run once".

## 🧩 Challenges Faced and Solutions

Getting this to work took many attempts, and every error taught me something new.

| Challenge | Solution |
|---|---|
| No records passed through the filter | Corrected the weekday comparison |
| `now is not a valid date` error | Configured the date expression correctly |
| Text Aggregator initially had no useful output | Fixed the upstream filter and tested again |
| Gmail displayed a placeholder instead of actual data | Inserted the mapped Text Aggregator output |
| Email content appeared on one line | Used HTML formatting to preserve line breaks |
| Extra spaces appeared between class details | Removed unnecessary blank lines from the aggregation template |
| Subject appeared in the wrong order | Changed the template to Subject → Start → End |
| Needed spacing between separate classes | Configured the row separator and HTML formatting |

## 🚀 Future Scope

This project can grow in many directions:

- **Telegram notifications:** Send timetable reminders through a Telegram bot.
- **WhatsApp reminders:** Deliver daily schedules through a supported WhatsApp integration.
- **Upcoming class alerts:** Send a notification shortly before each class starts.
- **Timetable change detection:** Notify students whenever the timetable is updated.
- **Holiday and event handling:** Skip reminders on configured holidays or college events.
- **AI-powered timetable assistant:** Integrate an LLM to answer natural-language questions about the timetable.
- **Natural-language queries:** Allow questions such as "What is my first class today?"
- **Multiple student support:** Handle separate timetables for multiple students.
- **Error notifications:** Alert the user when a scheduled run or email delivery fails.

## 📚 What I Learned

This was my first automation, and building it taught me:

- No-code workflow automation.
- Connecting applications through modules.
- Mapping data between modules.
- Conditional filtering and date functions.
- Combining multiple records with a Text Aggregator.
- HTML email formatting.
- Debugging and testing multi-step workflows.
- Scheduled automation and resource usage.

Most importantly, I learned that persistence pays off: a workflow that failed many times finally ran end to end.

