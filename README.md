# automated-report-generator
#it will read the data from a csv file and create a pdf file and write the read data into it 
from fpdf import FPDF
import pandas as p
#read data 
df=p.read_csv('data.csv')
#analyze data
s=df.describe(include='all')
department_counts=df['Department'].value_counts()
average_score=df['Score'].mean()
#generate pdf report
class PDF(FPDF):
    def header(self):
        self.set_font("Arial",'B',14)
        self.cell(200,10,"Employee data report",ln=True,align='C')
        self.ln(10)
    def footer(self):
        self.set_y(-15)
        self.set_font("Arial",'I',8)
        self.cell(0,10,f"Page {self.page_no()}",0,0,"C")
        
    def chapter_title(self, title):
        self.set_font("Arial", 'B', 12)
        self.cell(0, 10, title, ln=True)
        self.ln(5)    
    def chapter_body(self,text):
        self.set_font("Arial","",11)
        self.multi_cell(0,10,text)
        self.ln()
pdf=PDF()
pdf.add_page()
pdf.chapter_title("1.Summary Statistics")
summary_text=s.to_string()
pdf.chapter_body(summary_text)
pdf.chapter_title("2.Department Discription")
dept_text=department_counts.to_string()
pdf.chapter_body(dept_text)
pdf.chapter_title("3.Average Score")
pdf.chapter_body(f"Average Score across all employee: {average_score:.2f}")
pdf.output("sample_report.pdf")
print("Report generated sample_report.pdf")        
