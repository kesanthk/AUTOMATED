# AUTOMATED
import pandas as pd
from fpdf import FPDF

# Step 1: Load Data
df = pd.read_csv("data.csv")

# Step 2: Basic Analysis
total_sales = df["Sales"].sum()
average_sales = df["Sales"].mean()
top_seller = df.loc[df["Sales"].idxmax(), "Name"]

# Step 3: Generate PDF Report
class PDF(FPDF):
    def header(self):
        self.set_font("Arial", "B", 16)
        self.cell(0, 10, "Sales Report", ln=True, align="C")
        self.ln(10)

    def footer(self):
        self.set_y(-15)
        self.set_font("Arial", "I", 8)
        self.cell(0, 10, f"Page {self.page_no()}", 0, 0, "C")

pdf = PDF()
pdf.add_page()
pdf.set_font("Arial", size=12)

# Summary Section
pdf.cell(0, 10, f"Total Sales: {total_sales}", ln=True)
pdf.cell(0, 10, f"Average Sales: {average_sales:.2f}", ln=True)
pdf.cell(0, 10, f"Top Seller: {top_seller}", ln=True)
pdf.ln(10)

# Detailed Table
pdf.set_font("Arial", "B", 12)
pdf.cell(40, 10, "Name", 1)
pdf.cell(40, 10, "Sales", 1)
pdf.cell(40, 10, "Region", 1)
pdf.ln()

pdf.set_font("Arial", size=12)
for index, row in df.iterrows():
    pdf.cell(40, 10, row["Name"], 1)
    pdf.cell(40, 10, str(row["Sales"]), 1)
    pdf.cell(40, 10, row["Region"], 1)
    pdf.ln()

# Step 4: Save PDF
pdf.output("Sales_Report.pdf")
print("Report generated as 'Sales_Report.pdf'")