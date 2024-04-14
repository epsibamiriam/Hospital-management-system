# Hospital-management-system
from tkinter import*
from tkinter import ttk
import os
import tempfile
from twilio.rest import Client
root=Tk()
root.title("Hospital management System")
import mysql.connector
from tkinter import messagebox
root.state('zoomed')
root.config(bg='black')
#crteating function to access save prescription button and also creating for error message
def pd():
    if box_not.get()=="" or boxref.get()=="":
        messagebox.showerror("Error","All fields are required")
    else:
        con=mysql.connector.connect(host='localhost',username='python project1',password='mypassword',database='hospital')
        my_cursor=con.cursor()
        my_cursor.execute("insert into hospital values(%s,%s,%s,%s,%s,%s,%s,%s,%s,%s,%s)",(
        
            nameoftab.get(),
            ref.get(),
            dos.get(),
            nooftab.get(),
            issuedate.get(),
            expdate.get(),
            dailydose.get(),
            side_eff.get(),
            paitentid.get(),
            dob.get(),
            paitentadd.get()))
        con.commit()
        fetch_data()
        con.close()
        messagebox.showinfo("Successfull","The data hes been inserted successfully!")

# creating function to access prescription button and showing prescription
def prescription():
    txtpres.insert(END,'Name of the paitent:\t\t\t'+paitentid.get()+'\n')
    txtpres.insert(END,'Date of birth:\t\t\t'+dob.get()+'\n')
    txtpres.insert(END,"Paitent's number:\t\t\t"+paitentnum.get()+'\n')
    txtpres.insert(END,"Paitent's address:\t\t\t"+ paitentadd.get()+'\n')
    txtpres.insert(END,'Reference number:\t\t\t'+ref.get()+'\n')
    txtpres.insert(END,'Blood pressure:\t\t\t'+bp.get()+'\n')
    txtpres.insert(END,'Dose(mg):\t\t\t'+dos.get()+'\n')
    txtpres.insert(END,'Daily dose:\t\t\t'+dailydose.get()+'\n')
    txtpres.insert(END,'Name of the tablets:\t\t\t'+nameoftab.get()+'\n')
    txtpres.insert(END,'Number of tablets:\t\t\t'+nooftab.get()+'\n')
    txtpres.insert(END,'Issue date:\t\t\t'+issuedate.get()+'\n')
    txtpres.insert(END,'Further information:\t\t\t'+further_info.get()+'\n')
    
# For displaying the datails stored in the database
def fetch_data():
    con=mysql.connector.connect(host='localhost',username='python project1',password='mypassword',database='hospital')
    my_cursor=con.cursor()
    my_cursor.execute('select * from hospital')
    rows=my_cursor.fetchall()
    if len(rows)!=0:
        table.delete(*table.get_children())
        for items in rows:
            table.insert('',END,values=items)
        con.commit()
    con.close()

#fetching and displaying the details from database table
def get_data(event=''):
    cursor_row=table.focus()
    data=table.item(cursor_row)
    row=data['values']
    nameoftab.set(row[0])
    ref.set(row[1])
    dos.set(row[2])
    nooftab.set(row[3])
    issuedate.set(row[4])
    expdate.set(row[5])
    dailydose.set(row[6])
    side_eff.set(row[7])
    paitentid.set(row[8])
    dob.set(row[9])
    paitentadd.set(row[10])

#creating function to access delete button
def delete():
    con=mysql.connector.connect(host='localhost',username='python project1',password='mypassword',database='hospital')
    my_cursor=con.cursor()
    deleting=('delete from hospital where reference =%s')
    value=(ref.get(),)
    my_cursor.execute(deleting,value)
    con.commit()
    con.close()
    fetch_data()
    messagebox.showinfo('Information',"Paitent's data was deleted!")

#creating function for to access clear button
def clear():
    nameoftab.set('')
    ref.set('')
    dos.set('')
    nooftab.set('')
    lot.set('')
    issuedate.set('')
    expdate.set('')
    dailydose.set('')
    side_eff.set('')
    further_info.set('')
    bp.set('')
    stor_advi.set('')
    medi.set('')
    paitentid.set('')
    NHS.set('')
    paitentnum.set('')
    dob.set('')
    paitentadd.set('')
    
#creating function for to access exist button
def exist():
    confirm= messagebox .askyesno('Conformation','Are you sure do you want to exist?')
    if confirm>0:
        root.destroy()
        return
# function for print option 
def print_text(txt):
    print('helloooo')

#function for message option
def message():
    account_Sid = 'ACbc6e19269739aca0e09779cc1537e51c'
    auth_token = '99cd950e3916008815a24ddfc065c0b4'
    client = Client(account_Sid ,auth_token)

    message_text = txtpres.get('1.0', END)
    static_text = "\n\nFrom Birla hospital - Madurai\nThis is the Prescription of your today's health checkup\n\n"
    message_body = static_text + message_text

    message=client.messages\
       .create(
            body= message_body,
            from_='+12513021654',
            to= '+917824952342'
      )
    print(message.sid)
# function for print option
def print_text(text):
    temp_file=tempfile.mktemp('.txt')
    open(temp_file, 'w').write(text)
    os.startfile(temp_file, 'print')

#creating the heading 'hospital management system'
lbltitle=Label(root,bd=18,relief=RIDGE,text="Birla hospital pharmacy - Madurai",fg="green",bg="white",font=("times new romen",45,"bold"))
lbltitle.pack(side=TOP,fill=X)
#creating frame for filling space in (left side) and in prescription in (right side) and also in the data displaying (down) 
dataframe=Frame(root,bd=10, relief=RIDGE)
dataframe.place(x=0, y=115, width=1500, height=390)
dataframeleft=LabelFrame(dataframe, bd=10, relief=RIDGE, padx=10, font=("times new romen",12),text="Patient")
dataframeleft.place(x=0,y=5,width=917,height=368)
dataframeright=LabelFrame(dataframe, bd=10, relief=RIDGE, padx=10, font=("times new romen",12),text='Prescription')
dataframeright.place(x=920,y=5,width=400,height=390)
detailsframe=Frame(root, bd=10, relief=RIDGE)
detailsframe.place(x=0,y=510,width=1360,height=150)

#Text variable for all entry field
nameoftab= StringVar()
ref=StringVar()
dos=StringVar()
nooftab=StringVar()
lot=StringVar()
issuedate=StringVar()
expdate=StringVar()
dailydose=StringVar()
side_eff=StringVar()
further_info=StringVar()
bp=StringVar()
stor_advi=StringVar()
medi=StringVar()
paitentid=StringVar()
NHS=StringVar()
paitentnum=StringVar()
dob=StringVar()
paitentadd=StringVar()

#creating the names and the text box for Names of Tablet
Name_tablet=Label(dataframeleft,text="Names of Tablet:",font=('times new romen',12,"bold"),padx=2,pady=6)
Name_tablet.grid(row=0,column=0,sticky=W)
box_not=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32,textvar= nameoftab)
box_not.grid(row=0,column=1)
#creating the names and the text box for Reference      
lbref=Label(dataframeleft,text="Reference:",font=("times new romen",12,"bold"),textvar='Reference number:',padx=2)
lbref.grid(row=1, column=0,sticky=W)
boxref=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32, textvar=ref)
boxref.grid(row=1,column=1)
#creating the names and the text box for Dose   
lbdos=Label(dataframeleft,font=("times new romen",12,"bold"),text='Dose:',padx=2,pady=4)
lbdos.grid(row=2, column=0,sticky=W)
boxdos=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32,textvar= dos)
boxdos.grid(row=2,column=1)
#creating the names and the text box for No.of.Tablets  
lb_no_of_tablets=Label(dataframeleft,font=("times new romen",12,"bold"),text='No.of.Tablets:',padx=2,pady=4)
lb_no_of_tablets.grid(row=3, column=0,sticky=W)
box_no_of_tablets=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32,textvar=nooftab)
box_no_of_tablets.grid(row=3,column=1)
#creating the names and the text box for Lot 
lb_lot=Label(dataframeleft,font=("times new romen",12,"bold"),text='Lot:',padx=2,pady=4)
lb_lot.grid(row=4, column=0,sticky=W)
box_lot=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32,textvar=lot)
box_lot.grid(row=4,column=1)
#creating the names and the text box for Issue Date 
lb_issuedate=Label(dataframeleft,font=("times new romen",12,"bold"),text='Issue Date:',padx=2,pady=4)
lb_issuedate.grid(row=5, column=0,sticky=W)
box_issuedate=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32,textvar=issuedate)
box_issuedate.grid(row=5,column=1)
#creating the names and the text box for Expire Date 
lb_expdate=Label(dataframeleft,font=("times new romen",12,"bold"),text='Expire Date:',padx=2,pady=4)
lb_expdate.grid(row=6, column=0,sticky=W)
box_expdate=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32,textvar=expdate)
box_expdate.grid(row=6,column=1)
#creating the names and the text box for Daily Dose 
lb_Dailydose=Label(dataframeleft,font=("times new romen",12,"bold"),text='Daily Dose:',padx=2,pady=4)
lb_Dailydose.grid(row=7, column=0,sticky=W)
box_Dailydose=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32,textvar=dailydose)
box_Dailydose.grid(row=7,column=1)
#creating the names and the text box for Side Effect 
lb_sideeffect=Label(dataframeleft,font=("times new romen",12,"bold"),text='Side Effect:',padx=2,pady=4)
lb_sideeffect.grid(row=8, column=0,sticky=W)
box_sideeffect=Entry(dataframeleft,font=("times new romen",12,"bold"),width=32,textvar=side_eff)
box_sideeffect.grid(row=8,column=1)
#creating the names and the text box for Further Information 
lb_furtherinfo=Label(dataframeleft,font=("times new romen",12,"bold"),text='Further Information:',padx=2,pady=2)
lb_furtherinfo.grid(row=0, column=4,sticky=W)
box_furtherinfo=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=further_info)
box_furtherinfo.grid(row=0,column=5)
#creating the names and the text box for Blood Pressure
lb_bloodpressure=Label(dataframeleft,font=("times new romen",12,"bold"),text='Blood Pressure:',padx=2,pady=2)
lb_bloodpressure.grid(row=1, column=4,sticky=W)
box_bloodpressure=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=bp)
box_bloodpressure.grid(row=1,column=5)
#creating the names and the text box for storage advice
lb_storageadvice=Label(dataframeleft,font=("times new romen",12,"bold"),text='Storage Advice:',padx=2,pady=2)
lb_storageadvice.grid(row=2, column=4,sticky=W)
box_storageadvice=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=stor_advi)
box_storageadvice.grid(row=2,column=5)
#creating the names and the text box for medication
lb_medication=Label(dataframeleft,font=("times new romen",12,"bold"),text='Medication:',padx=2,pady=2)
lb_medication.grid(row=3, column=4,sticky=W)
box_medication=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=medi)
box_medication.grid(row=3,column=5)
#creating the names and the text box for Paitent name
lb_patient_id=Label(dataframeleft,font=("times new romen",12,"bold"),text="Patient's name:",padx=2,pady=2)
lb_patient_id.grid(row=4, column=4,sticky=W)
box_patient_id=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=paitentid)
box_patient_id.grid(row=4,column=5)
#creating the names and the text box for NHS number
lb_NHS_num=Label(dataframeleft,font=("times new romen",12,"bold"),text="NHS number:",padx=2,pady=2)
lb_NHS_num.grid(row=5, column=4,sticky=W)
box_NHS_num=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=NHS)
box_NHS_num.grid(row=5,column=5)
#creating the names and the text box for paitent number
lb_paitents_num=Label(dataframeleft,font=("times new romen",12,"bold"),text="paitents number:",padx=2,pady=2)
lb_paitents_num.grid(row=6, column=4,sticky=W)
box_paitents_num=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=paitentnum)
box_paitents_num.grid(row=6,column=5)
#creating the names and the text box for DOB   
lb_DOB=Label(dataframeleft,font=("times new romen",12,"bold"),text="Date Of Birth:",padx=2,pady=2)
lb_DOB.grid(row=7, column=4,sticky=W)
box_DOB=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=dob)
box_DOB.grid(row=7,column=5)
#creating the names and the text box for paitent address
lb_paitent_add=Label(dataframeleft,font=("times new romen",12,"bold"),text="Paitent's address:",padx=2,pady=2)
lb_paitent_add.grid(row=8, column=4,sticky=W)
box_paitent_add=Entry(dataframeleft,font=("times new romen",12,"bold"),width=30,textvar=paitentadd)
box_paitent_add.grid(row=8,column=5)
#Text box for precription diplaying on right side
txtpres=Text(dataframeright, font=('times new romen',12,"bold"),width=41,height=17,padx=0,pady=5.5)
txtpres.grid(row=0,column=0)
#Creating button
#prescription button
pres_but=Button(root, text="prescription",font="ariel 15 bold",bg="red",fg='white',bd=2,cursor="hand2",command=prescription)
pres_but.place(x=3,y=660,width=320)
#save prescription data
save_pres_but=Button(root, text="save prescription data",font="ariel 15 bold",bg="green",fg='white',bd=2,cursor="hand2",command=pd)
save_pres_but.place(x=320,y=660,width=300)
#delete button
del_but=Button(root, text="Delete",font="ariel 15 bold",bg="blue",fg='white',bd=2,cursor="hand2",command=delete)
del_but.place(x=600,y=660,width=300)
#clear button
clear_but=Button(root, text="Clear",font="ariel 15 bold",bg="yellow",fg='black',bd=2,cursor="hand2",command=clear)
clear_but.place(x=870,y=660,width=280)
#exist button
exist_but=Button(root, text="Exist",font="ariel 15 bold",bg="brown",fg='white',bd=2,cursor="hand2",command=exist)
exist_but.place(x=1125,y=660,width=235)
#print button
print_button = Button(root, text='Print', command=lambda: print_text(txtpres.get('1.0', END)))
print_button.place(x=850, y=450)
#message button
messbutton = Button(root, text='Message',command=message)
messbutton.place(x=700, y=450,width=100)
#For scrolling
scroll_1=ttk.Scrollbar(detailsframe,orient=HORIZONTAL)
scroll_1.pack(side='bottom',fill='x')
scroll_2=ttk.Scrollbar(detailsframe,orient=VERTICAL)
scroll_2.pack(side='right',fill='y')
table=ttk.Treeview(detailsframe,column=('not','ref','dose','no.of tab','issue','exp','dd','sd','pn','dob','pa'))

#headings for prescription data
table.heading('not',text='Name of tablet')
table.heading('ref',text='Reference number')
table.heading('dose',text='Dose')
table.heading('no.of tab',text='No.of.tablet')
table.heading('issue',text='Issue Date')
table.heading('exp',text='Exp Date')
table.heading('dd',text='Daily dose')
table.heading('sd',text='Side effect')
table.heading('pn',text='Paitent name')
table.heading('dob',text='DOB')
table.heading('pa',text='Patient address')
#To get the details in the precription 
table['show']='headings'
table.pack(fill=BOTH,expand=1)
# Creating columns in the data shows below 
table.column('not',width=95)
table.column('ref',width=95)
table.column('dose',width=95)
table.column('no.of tab',width=95)
table.column('issue',width=95)
table.column('exp',width=95)
table.column('dd',width=95)
table.column('sd',width=95)
table.column('pn',width=95)
table.column('dob',width=95)
table.column('pa',width=95)
# To display the information in the text box whatever is below
table.bind('<ButtonRelease-1>',get_data)
fetch_data()
#Closing
root.mainloop()
