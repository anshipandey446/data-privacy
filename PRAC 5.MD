DATA PRIVACY PRACTICAL
Practical No. 5
Title

Implementation of Data Anonymization Techniques Using Amazon Sample Data

Aim

To study and implement data anonymization techniques such as k-anonymity, differential privacy, and data masking using a sample Amazon e-commerce dataset.

Objectives

To understand data anonymization.

To protect sensitive customer information.

To implement k-anonymity.

To demonstrate differential privacy.

To implement data masking.

To understand the importance of privacy in e-commerce data.

1. Introduction

E-commerce websites store large amounts of customer and order information. Such data may contain sensitive information such as customer names, email addresses, locations, and purchase details.

Data anonymization modifies or removes sensitive information so that data can be analyzed while reducing privacy risks.

In this practical, we use a fictional Amazon-style dataset and apply:

K-Anonymity

Differential Privacy

Data Masking

2. Amazon Sample Dataset

The following is a fictional dataset created for this practical.

Order_ID	Customer	Age	City	Email	Product	Category	Price	Rating
A001	Rahul	21	Delhi	rahul@gmail.com	Laptop	Electronics	55000	4.5
A002	Priya	22	Delhi	priya@gmail.com	Headphones	Electronics	2500	4.2
A003	Aman	25	Mumbai	aman@gmail.com	Smartphone	Electronics	30000	4.4
A004	Neha	26	Mumbai	neha@gmail.com	Smartwatch	Electronics	5000	4.1
A005	Rohit	31	Delhi	rohit@gmail.com	Shoes	Fashion	3000	4.0
A006	Anjali	32	Delhi	anjali@gmail.com	Handbag	Fashion	2500	4.3
A007	Karan	23	Delhi	karan@gmail.com	Keyboard	Electronics	1800	4.5
A008	Pooja	24	Mumbai	pooja@gmail.com	Dress	Fashion	2200	4.1
3. Requirements
Software

Python 3.x

Pandas

NumPy

Jupyter Notebook / Google Colab / VS Code

Installation
pip install pandas numpy

4. Create the Amazon Dataset
Program
import pandas as pd

data = {
    "Order_ID": [
        "A001", "A002", "A003", "A004",
        "A005", "A006", "A007", "A008"
    ],

    "Customer": [
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

    "Product": [
        "Laptop",
        "Headphones",
        "Smartphone",
        "Smartwatch",
        "Shoes",
        "Handbag",
        "Keyboard",
        "Dress"
    ],

    "Category": [
        "Electronics",
        "Electronics",
        "Electronics",
        "Electronics",
        "Fashion",
        "Fashion",
        "Electronics",
        "Fashion"
    ],

    "Price": [
        55000, 2500, 30000, 5000,
        3000, 2500, 1800, 2200
    ],

    "Rating": [
        4.5, 4.2, 4.4, 4.1,
        4.0, 4.3, 4.5, 4.1
    ]
}

df = pd.DataFrame(data)

print("Original Amazon Dataset:")
print(df)

Output
  Order_ID Customer  Age    City             Email      Product
0     A001   Rahul   21   Delhi     rahul@gmail.com       Laptop
1     A002   Priya   22   Delhi     priya@gmail.com   Headphones
2     A003    Aman   25  Mumbai      aman@gmail.com   Smartphone
3     A004    Neha   26  Mumbai       neha@gmail.com  Smartwatch
4     A005   Rohit   31   Delhi     rohit@gmail.com        Shoes
5     A006  Anjali   32   Delhi    anjali@gmail.com      Handbag
6     A007   Karan   23   Delhi     karan@gmail.com     Keyboard
7     A008   Pooja   24  Mumbai     pooja@gmail.com        Dress

5. K-Anonymity
Theory

K-anonymity reduces the possibility of identifying a person by making records similar based on selected attributes.

Here, we use k = 2.

We remove direct identifiers such as:

Customer name

Order ID

Email

We also generalize age into age groups.

For example:

21 → 20-30
22 → 20-30
31 → 31-40
32 → 31-40

Program
# Create age groups
df["Age_Group"] = pd.cut(
    df["Age"],
    bins=[20, 30, 40],
    labels=["20-30", "31-40"]
)

# Remove direct identifiers
k_data = df[
    ["Age_Group", "City", "Category", "Price"]
]

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

  Age_Group    City     Category  Price
0     20-30   Delhi  Electronics  55000
1     20-30   Delhi  Electronics   2500
2     20-30  Mumbai  Electronics  30000
3     20-30  Mumbai  Electronics   5000
4     31-40   Delhi       Fashion   3000
5     31-40   Delhi       Fashion   2500
6     20-30   Delhi  Electronics   1800
7     20-30  Mumbai       Fashion   2200

Group Counts:

Age_Group  City
20-30      Delhi     3
           Mumbai    3
31-40      Delhi     2

Result

The direct identifiers were removed and the exact ages were generalized into age groups. Each selected age-group/city combination has at least two records, demonstrating 2-anonymity for those attributes.

6. Data Masking
Theory

Data masking hides part of sensitive information.

For example:

Original: rahul@gmail.com
Masked:   ra***@gmail.com


This allows the data to be displayed without revealing the complete email address.

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

print("Masked Customer Emails:")

print(
    df[["Customer", "Email", "Masked_Email"]]
)

Output
  Customer             Email       Masked_Email
0   Rahul     rahul@gmail.com     ra***@gmail.com
1   Priya     priya@gmail.com     pr***@gmail.com
2   Aman       aman@gmail.com      am**@gmail.com
3   Neha       neha@gmail.com      ne**@gmail.com
4   Rohit     rohit@gmail.com     ro***@gmail.com
5   Anjali   anjali@gmail.com     an****@gmail.com
6   Karan     karan@gmail.com     ka***@gmail.com
7   Pooja     pooja@gmail.com     po***@gmail.com

Result

The email addresses were successfully masked to protect customer information.

7. Differential Privacy
Theory

Differential privacy protects individual information by adding controlled random noise to a statistical result.

In this practical, we calculate the average product price and add random noise.

Actual Average → Add Noise → Privacy-Protected Average

Program
import numpy as np

# Calculate actual average price
actual_average = df["Price"].mean()

# Generate random noise
noise = np.random.normal(0, 1000)

# Add noise
private_average = actual_average + noise

print(
    "Actual Average Price:",
    round(actual_average, 2)
)

print(
    "Private Average Price:",
    round(private_average, 2)
)

Sample Output
Actual Average Price: 12625.0
Private Average Price: 13412.56


The private value can be different each time because random noise is generated.

Result

Random noise was added to the average product price, demonstrating the basic idea of differential privacy.

Note: This is a simplified educational demonstration. Real differential-privacy systems use formal privacy parameters and carefully calibrated noise.

8. Final Anonymized Dataset

After applying the anonymization techniques, sensitive information can be represented as:

Age Group	City	Category	Masked Email	Price
20-30	Delhi	Electronics	ra***@gmail.com	55000
20-30	Delhi	Electronics	pr***@gmail.com	2500
20-30	Mumbai	Electronics	am**@gmail.com	30000
20-30	Mumbai	Electronics	ne**@gmail.com	5000
31-40	Delhi	Fashion	ro***@gmail.com	3000
31-40	Delhi	Fashion	an****@gmail.com	2500
20-30	Delhi	Electronics	ka***@gmail.com	1800
20-30	Mumbai	Fashion	po***@gmail.com	2200
9. Comparison of Techniques
Technique	Purpose	Applied To
K-Anonymity	Makes records less distinguishable	Age and City
Differential Privacy	Adds noise to statistical results	Average Price
Data Masking	Hides sensitive information	Customer Email
10. Applications in E-Commerce

Anonymization can be used in e-commerce for:

Customer data analysis

Product recommendation research

Sales analysis

Market research

Fraud analysis

Machine learning

Business intelligence

Sharing datasets with researchers

11. Advantages

Protects customer information.

Reduces privacy risks.

Allows useful data analysis.

Helps protect sensitive customer details.

Makes data sharing safer.

12. Limitations

Anonymization does not guarantee complete anonymity.

Anonymized data can sometimes be re-identified using additional information.

Too much anonymization can reduce the usefulness of data.

Different datasets require different privacy techniques.

13. Conclusion

In this practical, a fictional Amazon-style e-commerce dataset was created and three data anonymization techniques were applied.

K-anonymity was used to generalize customer ages and remove direct identifiers. Data masking was used to hide portions of customer email addresses. Differential privacy was demonstrated by adding random noise to the average product price.

These techniques demonstrate how sensitive e-commerce data can be processed while reducing privacy risks.

