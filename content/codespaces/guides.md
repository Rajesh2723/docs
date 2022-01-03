// Retainers table 
#!/usr/bin/env python
# coding: utf-8

# in[ ]:


#---------------------------------------->  employee management system   <-----------------------------------------------------#

########################################    defining addemployee   #############################################
def addemployee():
    
    #<<<------------- defining the submit button in addemployee form ------------->>>#
    def submitadd():
        id = idval.get()                      #getting data from entries...or entry lables
        name = nameval.get()
        mobile = mobileval.get()
        email = emailval.get()
        address = addressval.get()
        gender = genderval.get()
        salary = salaryval.get()
        addedtime = time.strftime("%h:%m:%s") #to get time
        addeddate = time.strftime("%d/%m/%y") #to get date
        try:
            strr = 'insert into employeedata1 values(%s,%s,%s,%s,%s,%s,%s,%s,%s)'
            mycursor.execute(strr, (id, name, mobile, email, address, gender, salary, addeddate, addedtime))
            con.commit()
            res = messagebox.askyesnocancel('notificatrions','id {} name {} added sucessfully.. and want to clean the form'.format(id,name),
                                            parent=addroot)
            
            # clearing the entry form after adding details to database only if res==true
            if (res == true):
                idval.set('')
                nameval.set('')
                mobileval.set('')
                emailval.set('')
                addressval.set('')
                genderval.set('')
                salaryval.set('')
        except:
            messagebox.showerror('notifications', 'id already exist try another id...', parent=addroot)
        strr = 'select * from employeedata1'
        mycursor.execute(strr)
        datas = mycursor.fetchall()
        employeetable.delete(*employeetable.get_children())
        for i in datas:
            vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]] #making data as list to print on treeview
            employeetable.insert('', end, values=vv)                    #to show data,employeetable treeview ,prints from starting to end of (vv) values
            
        #<<<--------------- end of defining sumbitbutton of addemployee --------------->>>#
        
    #<<<------------------- creating window for employeedata entry --------------------->>>#    
    addroot = toplevel(master=dataentryframe)
    addroot.grab_set()
    addroot.geometry('470x470+220+200')
    addroot.title('enter details')
    addroot.config(bg='light blue')
    addroot.resizable(false, false) #making window nonresizable
    
    # ------------------------- addemployee labels
    
    idlabel = label(addroot, text='enter id : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                    borderwidth=3, width=12, anchor='w')
    idlabel.place(x=10, y=10)

    namelabel = label(addroot, text='enter name : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                      borderwidth=3, width=12, anchor='w')
    namelabel.place(x=10, y=70)

    mobilelabel = label(addroot, text='enter mobile : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                        borderwidth=3, width=12, anchor='w')
    mobilelabel.place(x=10, y=130)

    emaillabel = label(addroot, text='enter email : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                       borderwidth=3, width=12, anchor='w')
    emaillabel.place(x=10, y=190)

    addresslabel = label(addroot, text='enter address: ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                         borderwidth=3, width=12, anchor='w')
    addresslabel.place(x=10, y=250)  # dcae96

    genderlabel = label(addroot, text='enter gender : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                        borderwidth=3, width=12, anchor='w')
    genderlabel.place(x=10, y=310)

    salarylabel = label(addroot, text='enter salary : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                        borderwidth=3, width=12, anchor='w')
    salarylabel.place(x=10, y=370)

    #------------------- addemployee entry
    idval = stringvar()
    nameval = stringvar()
    mobileval = stringvar()
    emailval = stringvar()
    addressval = stringvar()
    genderval = stringvar()
    salaryval = stringvar()

    #------------------- addemployee entry lables
    identry = entry(addroot, font=('helvetica', 15, 'bold'), bd=3, textvariable=idval, width=18)
    identry.place(x=250, y=10)

    nameentry = entry(addroot, font=('helvetica', 15, 'bold'), bd=3, textvariable=nameval, width=18)
    nameentry.place(x=250, y=70)

    mobileentry = entry(addroot, font=('helvetica', 15, 'bold'), bd=3, textvariable=mobileval, width=18)
    mobileentry.place(x=250, y=130)

    emailentry = entry(addroot, font=('helvetica', 15, 'bold'), bd=3, textvariable=emailval, width=18)
    emailentry.place(x=250, y=190)

    addressentry = entry(addroot, font=('helvetica', 15, 'bold'), bd=3, textvariable=addressval, width=18)
    addressentry.place(x=250, y=250)

    genderentry = entry(addroot, font=('helvetica', 15, 'bold'), bd=3, textvariable=genderval, width=18)
    genderentry.place(x=250, y=310)

    salaryentry = entry(addroot, font=('helvetica', 15, 'bold'), bd=3, textvariable=salaryval, width=18)
    salaryentry.place(x=250, y=370)
    
    #------------------------- addemployee submit button
    submitbtn = button(addroot, text="submit", bd=3, font=("bodoni", 16, "bold"), width=10)
    submitbtn.config(relief="raised", bg="#f7e7ce", activebackground="sky blue", command=submitadd)
    submitbtn.place(x=150, y=420)

    addroot.mainloop()
########################################    end of addemployee   #############################################

########################################   defining searchemployee   #########################################
def searchemployee():
    
    #<<<------------- defining the search button in searchemployee form ------------->>>#
    def search():
        id = idval.get()
        name = nameval.get()
        mobile = mobileval.get()
        email = emailval.get()
        address = addressval.get()
        gender = genderval.get()
        salary = salaryval.get()
        addeddate = time.strftime("%d/%m/%y")
        
        #<<<--------------- code for searching records ------------->>>#
        if (id != ''):                                       
            strr = 'select *from employeedata1 where id=%s'    #it will select all records with the entered value
            mycursor.execute(strr, (id))                       #executing that statement
            datas = mycursor.fetchall()                        #it will fetch all records and stores in datas
            employeetable.delete(*employeetable.get_children())# it will clear the console(showdataframe)
            for i in datas:
                vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]#it will print the fetched data of the searched record on the console
                employeetable.insert('', end, values=vv)  # it will print fetched record from starting to end of that value
        elif (name != ''):
            strr = 'select *from employeedata1 where name=%s'
            mycursor.execute(strr, (name))
            datas = mycursor.fetchall()
            employeetable.delete(*employeetable.get_children())
            for i in datas:
                vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
                employeetable.insert('', end, values=vv)
        elif (mobile != ''):
            strr = 'select *from employeedata1 where mobile=%s'
            mycursor.execute(strr, (mobile))
            datas = mycursor.fetchall()
            employeetable.delete(*employeetable.get_children())
            for i in datas:
                vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
                employeetable.insert('', end, values=vv)
        elif (email != ''):
            strr = 'select *from employeedata1 where email=%s'
            mycursor.execute(strr, (email))
            datas = mycursor.fetchall()
            employeetable.delete(*employeetable.get_children())
            for i in datas:
                vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
                employeetable.insert('', end, values=vv)
        elif (address != ''):
            strr = 'select *from employeedata1 where address=%s'
            mycursor.execute(strr, (address))
            datas = mycursor.fetchall()
            employeetable.delete(*employeetable.get_children())
            for i in datas:
                vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
                employeetable.insert('', end, values=vv)
        elif (gender != ''):
            strr = 'select *from employeedata1 where gender=%s'
            mycursor.execute(strr, (gender))
            datas = mycursor.fetchall()
            employeetable.delete(*employeetable.get_children())
            for i in datas:
                vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
                employeetable.insert('', end, values=vv)
        elif (salary != ''):
            strr = 'select *from employeedata1 where salary=%s'
            mycursor.execute(strr, (salary))
            datas = mycursor.fetchall()
            employeetable.delete(*employeetable.get_children())
            for i in datas:
                vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
                employeetable.insert('', end, values=vv)

        elif (addeddate != ''):
            strr = 'select *from employeedata1 where addeddate=%s'
            mycursor.execute(strr, (addeddate))
            datas = mycursor.fetchall()
            employeetable.delete(*employeetable.get_children())
            for i in datas:
                vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
                employeetable.insert('', end, values=vv)
                
    #<<<--------- end of searching records code ---------->>>#
    
    #<<<--------- creating a window of searching from ----------->>>#
    searchroot = toplevel(master=dataentryframe)
    searchroot.grab_set()
    searchroot.geometry('470x540+220+200')
    searchroot.title('search records')
    searchroot.config(bg='light blue')
    searchroot.resizable(false, false)
    
    # --------------------------------search employee labels
    idlabel = label(searchroot, text='enter id : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove, borderwidth=3, width=12, anchor='w')
    idlabel.place(x=10, y=10)

    namelabel = label(searchroot, text='enter name : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,borderwidth=3, width=12, anchor='w')
    namelabel.place(x=10, y=70)

    mobilelabel = label(searchroot, text='enter mobile : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,borderwidth=3, width=12, anchor='w')
    mobilelabel.place(x=10, y=130)

    emaillabel = label(searchroot, text='enter email : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,borderwidth=3, width=12, anchor='w')
    emaillabel.place(x=10, y=190)

    addresslabel = label(searchroot, text='enter address: ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,borderwidth=3, width=12, anchor='w')
    addresslabel.place(x=10, y=250)  # dcae96

    genderlabel = label(searchroot, text='enter gender : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,borderwidth=3, width=12, anchor='w')
    genderlabel.place(x=10, y=310)

    salarylabel = label(searchroot, text='enter salary : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,borderwidth=3, width=12, anchor='w')
    salarylabel.place(x=10, y=370)

    datelabel = label(searchroot, text='enter date : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,borderwidth=3, width=12, anchor='w')
    datelabel.place(x=10, y=430)

    #----------------------------------search employee entry
    idval = stringvar()
    nameval = stringvar()
    mobileval = stringvar()
    emailval = stringvar()
    addressval = stringvar()
    genderval = stringvar()
    salaryval = stringvar()
    dateval = stringvar()

    #-----------------------------------saerch employee entrylables
    identry = entry(searchroot, font=('arial', 13, 'bold'), bd=5, textvariable=idval)
    identry.place(x=250, y=10)

    nameentry = entry(searchroot, font=('arial', 13, 'bold'), bd=5, textvariable=nameval)
    nameentry.place(x=250, y=70)

    mobileentry = entry(searchroot, font=('arial', 13, 'bold'), bd=5, textvariable=mobileval)
    mobileentry.place(x=250, y=130)

    emailentry = entry(searchroot, font=('arial', 13, 'bold'), bd=5, textvariable=emailval)
    emailentry.place(x=250, y=190)

    addressentry = entry(searchroot, font=('arial', 13, 'bold'), bd=5, textvariable=addressval)
    addressentry.place(x=250, y=250)

    genderentry = entry(searchroot, font=('arial', 13, 'bold'), bd=5, textvariable=genderval)
    genderentry.place(x=250, y=310)

    salaryentry = entry(searchroot, font=('arial', 13, 'bold'), bd=5, textvariable=salaryval)
    salaryentry.place(x=250, y=370)

    dateentry = entry(searchroot, font=('arial', 13, 'bold'), bd=5, textvariable=dateval)
    dateentry.place(x=250, y=430)
    
    #------------------------- search button
    searchbtn = button(searchroot, text='search', bd=3, font=("bodoni", 16, "bold"), width=10, command=search)
    searchbtn.config(relief="raised", bg="#f7e7ce", activebackground="sky blue")
    searchbtn.place(x=150, y=480)

    searchroot.mainloop()

########################################   end of defining searchemployee   #########################################

########################################  defining delete employee   ################################################
def deleteemployee():
    cc = employeetable.focus()         #focus on the record which we click and gets data of that record
    content = employeetable.item(cc)
    pp = content['values'][0]          #it gives the id of the particular record to delete 
    strr = 'delete from employeedata1 where id=%s'
    mycursor.execute(strr, (pp))
    con.commit()
    messagebox.showinfo('notifications', 'id {} deleted sucessfully...'.format(pp))
    strr = 'select *from employeedata1'
    mycursor.execute(strr)
    datas = mycursor.fetchall()
    employeetable.delete(*employeetable.get_children())
    for i in datas:
        vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
        employeetable.insert('', end, values=vv)

#######################################  end of deleteemployee   ####################################################

######################################   defining updateemployee  ###################################################
def updateemployee():
    
    #<<<--------------- defining updatebutton in update entry form -------------->>>#
    def update():
        id = idval.get()
        name = nameval.get()
        mobile = mobileval.get()
        email = emailval.get()
        address = addressval.get()
        gender = genderval.get()
        salary = salaryval.get()
        date = dateval.get()
        time = timeval.get()

        strr = 'update employeedata1 set name=%s,mobile=%s,email=%s,address=%s,gender=%s,salary=%s,date=%s,time=%s where id=%s'
        mycursor.execute(strr, (name, mobile, email, address, gender, salary, date, time, id))
        con.commit()
        messagebox.showinfo('notifications', 'id {} modified sucessfully...'.format(id), parent=updateroot)
        strr = 'select *from employeedata1'
        mycursor.execute(strr)
        datas = mycursor.fetchall()
        employeetable.delete(*employeetable.get_children())
        for i in datas:
            vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
            employeetable.insert('', end, values=vv)
    
    #<<<--------------- creating a window for updateentry form --------------->>>#
    updateroot = toplevel(master=dataentryframe)
    updateroot.grab_set()
    updateroot.geometry('470x585+220+160')
    updateroot.title('update record')
    updateroot.config(bg='light blue')
    updateroot.resizable(false, false)
    
    #---------------------------------- updateemployee labels
    idlabel = label(updateroot, text='enter id : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                    borderwidth=3, width=12, anchor='w')
    idlabel.place(x=10, y=10)

    namelabel = label(updateroot, text='enter name : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                      borderwidth=3, width=12, anchor='w')
    namelabel.place(x=10, y=70)

    mobilelabel = label(updateroot, text='enter mobile : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                        borderwidth=3, width=12, anchor='w')
    mobilelabel.place(x=10, y=130)

    emaillabel = label(updateroot, text='enter email : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                       borderwidth=3, width=12, anchor='w')
    emaillabel.place(x=10, y=190)

    addresslabel = label(updateroot, text='enter address: ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                         borderwidth=3, width=12, anchor='w')
    addresslabel.place(x=10, y=250)  # dcae96

    genderlabel = label(updateroot, text='enter gender : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                        borderwidth=3, width=12, anchor='w')
    genderlabel.place(x=10, y=310)

    salarylabel = label(updateroot, text='enter salary : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                        borderwidth=3, width=12, anchor='w')
    salarylabel.place(x=10, y=370)

    datelabel = label(updateroot, text='enter date : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                      borderwidth=3, width=12, anchor='w')
    datelabel.place(x=10, y=430)

    timelabel = label(updateroot, text='enter time : ', bg='#dcae96', font=('verdana', 17, 'bold'), relief=groove,
                      borderwidth=3, width=12, anchor='w')
    timelabel.place(x=10, y=490)

    #-------------------- updateemployee entry
    idval = stringvar()
    nameval = stringvar()
    mobileval = stringvar()
    emailval = stringvar()
    addressval = stringvar()
    genderval = stringvar()
    salaryval = stringvar()
    dateval = stringvar()
    timeval = stringvar()

    #---------------------------updateemployee entry labels
    
    identry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=idval)
    identry.place(x=250, y=10)

    nameentry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=nameval)
    nameentry.place(x=250, y=70)

    mobileentry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=mobileval)
    mobileentry.place(x=250, y=130)

    emailentry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=emailval)
    emailentry.place(x=250, y=190)

    addressentry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=addressval)
    addressentry.place(x=250, y=250)

    genderentry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=genderval)
    genderentry.place(x=250, y=310)

    salaryentry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=salaryval)
    salaryentry.place(x=250, y=370)

    dateentry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=dateval)
    dateentry.place(x=250, y=430)

    timeentry = entry(updateroot, font=('arial', 12, 'bold'), bd=5, textvariable=timeval)
    timeentry.place(x=250, y=490)
    
    #------------------------- update button
    updatebtn = button(updateroot, text="update", bd=3, font=("bodoni", 16, "bold"), width=10,command=update)
    updatebtn.config(relief="raised", bg="#f7e7ce", activebackground="sky blue")
    updatebtn.place(x=150, y=540)

    cc = employeetable.focus()      #this will get all details of existing record when clicked on it and fills in update entryform
    content = employeetable.item(cc)
    pp = content['values']
    if (len(pp) != 0):
        idval.set(pp[0])
        nameval.set(pp[1])
        mobileval.set(pp[2])
        emailval.set(pp[3])
        addressval.set(pp[4])
        genderval.set(pp[5])
        salaryval.set(pp[6])
        dateval.set(pp[7])
        timeval.set(pp[8])

    updateroot.mainloop()

########################################  end of defining updateemployee  ###############################################

########################################  defining show all records    ##################################################
def showallrecords():
    strr = 'select *from employeedata1'
    mycursor.execute(strr)
    datas = mycursor.fetchall()
    employeetable.delete(*employeetable.get_children())
    for i in datas:
        vv = [i[0], i[1], i[2], i[3], i[4], i[5], i[6], i[7], i[8]]
        employeetable.insert('', end, values=vv)

########################################  end of showallrecords  ########################################################

########################################  defining exitbutton    ########################################################
def exit_window():
    res = messagebox.askyesnocancel('notification', 'do you want to exit?')
    if (res == true):
        root.destroy()

########################################  end of exitbutton     ########################################################

##******************************************  defining logindatabase  ************************************************##
def connectdb():
    
    #--------------------> defining login button & making connection to database <-------------------#
    def submitdb():
        global con, mycursor
        host = "localhost"
        user = userval.get()
        password = passwordval.get()
        try:
            con = pymysql.connect(host=host, user=user, password=password)
            mycursor = con.cursor()
        except:
            messagebox.showerror('notifications', 'data is incorrect please try again', parent=dbroot)
            return
        try:
            strr = 'create database employeemanagementsystem1'
            mycursor.execute(strr)
            strr = 'use employeemanagementsystem1'
            mycursor.execute(strr)
            strr = 'create table employeedata1(id int,name varchar(20),mobile varchar(12),email varchar(30),address varchar(100),gender varchar(50),salary varchar(50),date varchar(50),time varchar(50))'
            mycursor.execute(strr)
            strr = 'alter table employeedata1 modify column id int not null'
            mycursor.execute(strr)
            strr = 'alter table employeedata1 modify column id int primary key'
            mycursor.execute(strr)
            messagebox.showinfo('notification',
                                'database created and now you are connected connected to the database ....',
                                parent=dbroot)

        except:
            strr = 'use employeemanagementsystem1'
            mycursor.execute(strr)
            messagebox.showinfo('notification', 'now you are connected to the database ....', parent=dbroot)
        dbroot.destroy()
    
    #--------------------> creating window for login database button <-----------------------#
    messagebox.showinfo('information..!','please login with mysql user id and password to create a database as localhost...!')
    dbroot = toplevel()
    dbroot.grab_set()
    dbroot.geometry("500x300+500+250")
    dbroot.resizable(false, false)
    dbroot.config(bg='light blue')
    dbroot.title("login to database")
    
    # ----------------------connectdb labels
    label1 = label(dbroot, text="login to database with mysql credentials", bd=4, bg="lavender", fg="red",
                   relief=solid, font=("courier", 14, "bold"))
    label1.pack(side="top", fill=both)

    userlabel = label(dbroot, text="mysql id :", font=("verdana", 12, "bold"),bg="lavender")
    userlabel.place(x=75, y=75)

    passwordlabel = label(dbroot, text="mysql password :", font=("verdana", 12, "bold"),bg="lavender")
    passwordlabel.place(x=75, y=150)

    # -----------------------connectdb entry
    userval = stringvar()
    passwordval = stringvar()

    #------------------------connectdb entrylabels
    userentry = entry(dbroot, textvariable=userval, bd=4, bg="white", relief="raised", width=57)
    userentry.place(x=75, y=110)

    passwordentry = entry(dbroot, textvariable=passwordval, bd=4, bg="white", relief="raised", width=57, show="*")
    passwordentry.place(x=75, y=185)

    # -----------------------login button & exit button
    login_button = button(dbroot, text="login", bd="4", bg="#dcae96", relief="raised", font=("arial", 11, "bold"),
                          command=submitdb)
    login_button.config(activebackground="red", activeforeground="white")
    login_button.place(x=75, y=235)

    exit_button = button(dbroot, text="exit", bd="4", bg="#dcae96", relief="raised", font=("arial", 11, "bold"),
                         command=dbroot.destroy)
    exit_button.config(activebackground="red", activeforeground="white")
    exit_button.place(x=165, y=235)

    dbroot.mainloop()

##****************************************** end of logindatabase button  ************************************************##

##*****************************************  defining time&date methods    ***********************************************##
def tick():
    time_string = time.strftime("%h:%m:%s")
    date_string = time.strftime("%d/%m/%y")
    clock.config(text='date :' + date_string + "\n" + "time : " + time_string)
    clock.after(200, tick)

##*****************************************  end of time&date methods    ************************************************##

##############################################  importing & creating mainwindow  ###############################################
from tkinter import *
from tkinter import toplevel, messagebox, filedialog
from tkinter.ttk import treeview
from tkinter import ttk
import pandas
import pymysql
import time

#---------------creating mainwindow
root = tk()
root.title("employee management system")
root.config(bg="light blue")
root.geometry("1100x700+200+50")
root.resizable(false, false)

####################################  creating two frames in main window   ############################################

#--------------------------------------------------- creating & developing data enrty frame 
dataentryframe = frame(root, bg="#dcae96", bd=1, relief="groove")
dataentryframe.place(x=20, y=80, width=500, height=600)

#--------------------data entry frame labels
frontlabel = label(dataentryframe, text="data management functions", relief="groove", bg="#f7e7ce", fg="navy blue",
                   font=("arial", 20, "bold"))
frontlabel.pack(side="top", fill=both)

addbtn = button(dataentryframe, text="add employee", relief="raised", bg="light blue", font=("verdana", 14, "bold"),
                width=20, command=addemployee)
addbtn.place(x=118, y=100)

updatebtn = button(dataentryframe, text="update record", relief="raised", bg="light blue", font=("verdana", 14, "bold"),
                   width=20, command=updateemployee)
updatebtn.place(x=118, y=180)

searchbtn = button(dataentryframe, text="search record", relief="raised", bg="light blue", font=("verdana", 14, "bold"),
                   width=20, command=searchemployee)
searchbtn.place(x=118, y=260)

delete_button = button(dataentryframe, text="delete record", relief="raised", bg="light blue",
                       font=("verdana", 14, "bold"), width=20, command=deleteemployee)
delete_button.place(x=118, y=340)

showall_button = button(dataentryframe, text="show all records", relief="raised", bg="light blue",
                        font=("verdana", 14, "bold"), width=20, command=showallrecords)
showall_button.place(x=118, y=420)

exit_button = button(dataentryframe, text="exit", relief="raised", bg="light blue", font=("verdana", 14, "bold"),
                     width=20, command=exit_window)
exit_button.place(x=118, y=500)

#--------------------------------------------------------------------end of data entry frame


#----------------------------------------------------craeting and developing show data frame
showdataframe = frame(root, bg='lavender', relief=groove, borderwidth=5)
showdataframe.place(x=580, y=80, width=500, height=600)

#-------------------making a table in showdataframe to get tree view of data
#-------------------making/creating treeview
style = ttk.style()
style.configure('treeview.heading', font=('verdana', 12, 'bold'), foreground='navy blue')
style.configure('treeview', font=('helvetica', 11, 'bold'), foreground='black', background='lavender')
scroll_x = scrollbar(showdataframe, orient=horizontal)
scroll_y = scrollbar(showdataframe, orient=vertical)
employeetable = treeview(showdataframe, columns=('id', 'name', 'mobile no', 'email', 'address', 'gender', 'salary', 'added date', 'added time'),
                         yscrollcommand=scroll_y.set, xscrollcommand=scroll_x.set)
scroll_x.pack(side=bottom, fill=x)
scroll_y.pack(side=right, fill=y)
scroll_x.config(command=employeetable.xview)
scroll_y.config(command=employeetable.yview)
employeetable.heading('id', text='id')
employeetable.heading('name', text='name')
employeetable.heading('mobile no', text='mobile no')
employeetable.heading('email', text='email')
employeetable.heading('address', text='address')
employeetable.heading('gender', text='gender')
employeetable.heading('salary', text='salary')
employeetable.heading('added date', text='added date')
employeetable.heading('added time', text='added time')
employeetable['show'] = 'headings'
employeetable.column('id', width=100)
employeetable.column('name', width=200)
employeetable.column('mobile no', width=200)
employeetable.column('email', width=300)
employeetable.column('address', width=200)
employeetable.column('gender', width=100)
employeetable.column('salary', width=150)
employeetable.column('added date', width=150)
employeetable.column('added time', width=150)
employeetable.pack(fill=both, expand=1)

#----------------------------------------------------------------------end of show data frame

#############################################  end of two frame developments  ###############################################

##################################### footer / top side of main window(root)  ###############################################

#------------------------------------ main label/heading
main_label = label(root, text="employee management system", relief="groove", bg="#fffdd0", fg="navy blue",
                   font=("arial", 22, "bold"))
main_label.pack(side="top")

#------------------------------------ date&time
clock = label(root, font=('verdana', 10, 'bold'), relief=raised, borderwidth=3, bg='lavender', fg="black")
clock.place(x=930, y=5)
tick()
#------------------------------------ login database button
connectbutton = button(root, text='login database', width=16, font=("verdana", 14, "bold"), command=connectdb)
connectbutton.config(bg="#dcae96", fg="black", activebackground="sky blue", activeforeground="black")
connectbutton.place(x=5, y=5)


root.mainloop()

#--------------------------------------------------->  end of code <-----------------------------------------------------------#

#all code was explained in detail in the report 
#done by:-
# k.uma maheshwar rao   rollno:-66
# g.sai rohith          rollno:-65
# m.dheeraj             rollno:-59


# in[ ]:





# in[ ]:



