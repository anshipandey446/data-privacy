# PRACTICAL 3


Implementation of Data Anonymization Techniques Using a Sample Dataset

Aim

To study and implement data anonymization techniques such as k-anonymity, differential privacy, and data masking using a sample student dataset.

Objectives

To understand the concept of data anonymization.

To protect sensitive information in a dataset.

To implement k-anonymity.

To implement a simple demonstration of differential privacy.

To implement data masking.

To understand the importance of anonymization in data privacy.

1. Introduction

Data anonymization is the process of modifying or removing information from a dataset to reduce the risk of identifying individuals.

In this practical, we use a fictional student dataset containing:

Student ID

Name

Age

City

Email

Marks

We apply three techniques:

K-Anonymity – Generalizes identifying information.

Differential Privacy – Adds random noise to statistical results.

Data Masking – Hides part of sensitive information.

2. Sample Dataset

The following fictional dataset is used for the practical.

Student_ID	Name	Age	City	Email	Marks
S001	Rahul	21	Delhi	rahul@gmail.com	85
S002	Priya	22	Delhi	priya@gmail.com	78
S003	Aman	25	Mumbai	aman@gmail.com	91
S004	Neha	26	Mumbai	neha@gmail.com	88
S005	Rohit	31	Delhi	rohit@gmail.com	74
S006	Anjali	32	Delhi	anjali@gmail.com	82
S007	Karan	23	Delhi	karan@gmail.com	89
S008	Pooja	24	Mumbai	pooja@gmail.com	76
3. Requirements
Software

Python 3.x

Pandas

NumPy

Jupyter Notebook / Google Colab / VS Code

Installation

Run:

pip install pandas numpy

4. Create the Dataset
Program
import pandas as pd

# Create sample dataset
data = {
    "Student_ID": [
        "S001", "S002", "S003", "S004",
        "S005", "S006", "S007", "S008"
    ],

    "Name": [
        "Rahul", "Priya", "Aman", "Neha",
        "Rohit", "Anjali", "Karan", "Pooja"
    ],

    "Age": [21, 22, 25, 26, 31, 32, 23, 24],

    "City": [
        "Delhi", "Delhi", "Mumbai", "Mumbai",
        "Delhi", "Delhi", "Delhi", "Mumbai"
    ],

    "Email": [
        "rahul@gmail.com",
        "priya@gmail.com",
        "aman@gmail.com",
        "neha@gmail.com",
        "rohit@gmail.com",
        "anjali@gmail.com",
        "karan@gmail.com",
        "pooja@gmail.com"
    ],

    "Marks": [85, 78, 91, 88, 74, 82, 89, 76]
}

df = pd.DataFrame(data)

print("Original Dataset:")
print(df)

Output
  Student_ID    Name  Age    City             Email  Marks
0       S001   Rahul   21   Delhi     rahul@gmail.com     85
1       S002   Priya   22   Delhi     priya@gmail.com     78
2       S003    Aman   25  Mumbai      aman@gmail.com     91
3       S004    Neha   26  Mumbai       neha@gmail.com     88
4       S005   Rohit   31   Delhi     rohit@gmail.com     74
5       S006  Anjali   32   Delhi    anjali@gmail.com     82
6       S007   Karan   23   Delhi     karan@gmail.com     89
7       S008   Pooja   24  Mumbai     pooja@gmail.com     76

Result

The sample student dataset was successfully created.

5. K-Anonymity
Theory

K-anonymity makes individuals less distinguishable by generalizing identifying information.

For this practical, we use k = 2.

We generalize the exact age into age groups:

21 → 20-30
22 → 20-30
31 → 31-40
32 → 31-40


We also remove direct identifiers such as Student ID and Name.

Program
# Create age groups
df["Age_Group"] = pd.cut(
    df["Age"],
    bins=[20, 30, 40],
    labels=["20-30", "31-40"]
)

# Remove direct identifiers
k_data = df[["Age_Group", "City", "Marks"]]

print("K-Anonymized Dataset:")
print(k_data)

# Count records in each group
print("\nGroup Counts:")

group_count = k_data.groupby(
    ["Age_Group", "City"],
    observed=True
).size()

print(group_count)

Output
K-Anonymized Dataset:

  Age_Group    City  Marks
0     20-30   Delhi     85
1     20-30   Delhi     78
2     20-30  Mumbai     91
3     20-30  Mumbai     88
4     31-40   Delhi     74
5     31-40   Delhi     82
6     20-30   Delhi     89
7     20-30  Mumbai     76

Group Counts:

Age_Group  City
20-30      Delhi     3
           Mumbai    3
31-40      Delhi     2


Since every group contains at least 2 records, the selected quasi-identifiers satisfy 2-anonymity in this simplified example.

Result

The exact ages and direct identifiers were removed/generalized, making individual students less distinguishable based on the selected attributes.

6. Data Masking
Theory

Data masking hides part of sensitive information while keeping some information visible.

For example:

Original: rahul@gmail.com
Masked:   ra***@gmail.com

Program
def mask_email(email):
    username, domain = email.split("@")

    if len(username) <= 2:
        masked = "*" * len(username)
    else:
        masked = username[:2] + "*" * (len(username) - 2)

    return masked + "@" + domain


# Apply masking
df["Masked_Email"] = df["Email"].apply(mask_email)

print("Masked Email Data:")

print(df[["Name", "Email", "Masked_Email"]])

Output
     Name             Email       Masked_Email
0   Rahul     rahul@gmail.com     ra***@gmail.com
1   Priya     priya@gmail.com     pr***@gmail.com
2    Aman      aman@gmail.com      am**@gmail.com
3    Neha       neha@gmail.com      ne**@gmail.com
4   Rohit     rohit@gmail.com     ro***@gmail.com
5  Anjali    anjali@gmail.com     an****@gmail.com
6   Karan     karan@gmail.com     ka***@gmail.com
7   Pooja     pooja@gmail.com     po***@gmail.com

Result

The sensitive portions of the email addresses were hidden using data masking.

7. Differential Privacy
Theory

Differential privacy protects individual information by adding controlled random noise to statistical results.

Here, we calculate the average marks and add random noise to it.

Actual Average → Add Noise → Privacy-Protected Average

Program
import numpy as np

# Calculate actual average
actual_average = df["Marks"].mean()

# Generate random noise
noise = np.random.normal(0, 2)

# Add noise
private_average = actual_average + noise

print("Actual Average Marks:",
      round(actual_average, 2))

print("Private Average Marks:",
      round(private_average, 2))

Sample Output
Actual Average Marks: 82.88
Private Average Marks: 84.12


The private average may be different each time because random noise is generated.

Result

Random noise was added to the average marks, demonstrating the basic idea of differential privacy.

Note: This is a simplified educational demonstration. A real differential-privacy system uses a formal privacy budget and carefully calibrated noise.

8. Final Anonymized Dataset

After applying anonymization, sensitive information can be represented as follows:

Age Group	City	Masked Email	Marks
20-30	Delhi	ra***@gmail.com	85
20-30	Delhi	pr***@gmail.com	78
20-30	Mumbai	am**@gmail.com	91
20-30	Mumbai	ne**@gmail.com	88
31-40	Delhi	ro***@gmail.com	74
31-40	Delhi	an****@gmail.com	82
20-30	Delhi	ka***@gmail.com	89
20-30	Mumbai	po***@gmail.com	76
9. Comparison
Technique	What it does	Example
K-Anonymity	Generalizes information	21 → 20-30
Differential Privacy	Adds noise to results	82.88 → 84.12
Data Masking	Hides sensitive information	rahul@gmail.com → ra***@gmail.com
10. Applications

Data anonymization is used in:

Healthcare

Banking

Education

Research

Government databases

Customer analytics

Machine learning

Business intelligence

11. Advantages

Protects personal information.

Reduces privacy risks.

Allows useful analysis of data.

Helps organizations share data more safely.

Reduces exposure of sensitive information.

12. Limitations

Anonymization does not always guarantee complete anonymity.

Anonymized data may sometimes be re-identified when combined with other information.

Too much anonymization can reduce the usefulness of data.

Different datasets require different privacy techniques.

13. Conclusion

In this practical, a sample student dataset containing personal information was created and three data anonymization techniques were implemented.

K-anonymity was demonstrated by generalizing exact ages into age groups. Data masking was used to hide portions of email addresses. Differential privacy was demonstrated by adding random noise to the average marks.

Thus, data anonymization techniques can help reduce privacy risks while allowing data to remain useful for analysis.

Viva Questions and Answers
1. What is data anonymization?

Data anonymization is the process of modifying data to reduce the possibility of identifying individuals.

2. What is k-anonymity?

K-anonymity makes each record indistinguishable from at least k-1 other records based on selected attributes.

3. What does k = 2 mean?

It means that each relevant combination of selected attributes should occur at least twice.

4. What is differential privacy?

Differential privacy is a mathematical approach that limits how much information about an individual can be learned from released data.

5. Why do we add noise in differential privacy?

Noise makes it harder to infer information about a particular individual from the released statistical result.

6. What is data masking?

Data masking hides part of sensitive information while keeping some information visible.

7. Give an example of data masking.
Original: 9876543210
Masked:   98******10

8. Why did we use age groups?

Age groups reduce the precision of the age information and make individual records less distinguishable.

9. Which library is used to create the dataset?

Pandas is used.

10. Which library is used for random noise?

NumPy is used.

11. Can anonymized data always be considered completely anonymous?

No. Re-identification can sometimes be possible if additional information is available.

12. Why is data anonymization important?

It helps reduce privacy risks when personal data is analyzed, stored, or shared.
