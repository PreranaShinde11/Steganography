from tkinter import *
from tkinter import messagebox
from tkinter import filedialog
import tkinter as tk
from PIL import Image , ImageTk
import os
from stegano import lsb
import ast
import openpyxl


root=Tk()
root.title("Login page")
root.geometry('925x500+300+200')
root.configure(bg="#08082b")
root.resizable(False,False)

image_icon1=PhotoImage(file="logo4.png")
root.iconphoto(False,image_icon1)

heading1=Label(root,text='WELCOME',fg='white',bg='#08082b',font=('Microsoft YaHei UI Light',23,'bold'))
heading1.place(x=155,y=20)

def signin():
    username=user.get()
    password=code.get()
    
    

    file=open('datasheet.txt','r')
    d=file.read()
    r=ast.literal_eval(d)
    file.close()

    #print(r.keys())
    #print(r.values())
    
    if username in r.keys() and password==r[username]:

        root.iconify()
        window=Toplevel()
        

        #=========================================================================================
        #=========================================================================================
        window.title("Steganography - Hide a secret Text Message in an Image")
        window.geometry("705x500+300+200")
        window.resizable(False,False)
        window.configure(bg="#2f4155")

        def showimage():
            global filename
            filename=filedialog.askopenfilename(initialdir=os.getcwd(),
                                                title='Select Image File',
                                                filetype=(("PNG file","*.png"),
                                                        ("JPG File","*.jpg"),
                                                        ("ALL file","*.txt")))

            img=Image.open(filename)
            img=ImageTk.PhotoImage(img)
            lb1.configure(image=img,width=250,height=250)
            lb1.image=img
            
        def Hide():
            global secret
            message=text1.get(1.0,END)
            secret = lsb.hide(str(filename), message)

        def Show():
            clear_message = lsb.reveal(filename)
            text1.delete(1.0,END)
            text1.insert(END, clear_message)


        def save():        
            path = "f2.xlsx"
            wb= openpyxl.load_workbook(path)
            sheet=wb.active

            
            b=sheet['B1'].value+1
            sheet['B1']=b
            
            wb.save("f2.xlsx")
            secret.save(f"{b}.png")  

            if messagebox.askyesno('confirmation','Image saved!! Do you want to save more encrypted data?') ==True:
                window.deincnify()
            else:
                window.destroy()


                


        #icon
        image_icon=PhotoImage(file="logo2.jpg")
        window.iconphoto(False,image_icon)


        #logo
        logo=PhotoImage(file="logo3.png")
        Label(window,image=logo,bg="#2f4155").place(x=10,y=0)

        Label(window,text="CYBER SECURITY",bg="#2d4155",fg="white",font="arial 25 bold ").place(x=100,y=20)

        #first frame
        f=Frame(window,bd=3,bg="black",width=340,height=280,relief=GROOVE)
        f.place(x=10,y=80)

        lb1=Label(f,bg="black")
        lb1.place(x=40,y=10)

        #second frame
        frame2=Frame(window,bd=3,width=340,height=280,bg="white",relief=GROOVE)
        frame2.place(x=350,y=80)

        text1=Text(frame2,font="Robote 20",bg="white",fg="black",relief=GROOVE)
        text1.place(x=0,y=0,width=320,height=295)

        scrollbar1=Scrollbar(frame2)
        scrollbar1.place(x=320,y=0,height=300)

        scrollbar1.configure(command=text1.yview)
        text1.configure(yscrollcommand=scrollbar1.set)

        #third frame
        frame3=Frame(window,bd=3,bg="#2f4155",width=330,height=100,relief=GROOVE)
        frame3.place(x=10,y=370)

        Button(frame3,text="Open Image",width=10,height=2,font="arial 14 bold",command=showimage).place(x=20,y=30)
        Button(frame3,text="Save Image",width=10,height=2,font="arial 14 bold",command=save).place(x=180,y=30)
        Label(frame3,text="Picture, Image,Photo file",bg="#2f4155",fg="yellow").place(x=20,y=5)

        #forth frame
        frame4=Frame(window,bd=3,bg="#2f4155",width=330,height=100,relief=GROOVE)
        frame4.place(x=360,y=370)

        Button(frame4,text="Hide Data",width=10,height=2,font="arial 14 bold",command=Hide).place(x=20,y=30)
        Button(frame4,text="Show Data",width=10,height=2,font="arial 14 bold",command=Show).place(x=180,y=30)
        Label(frame4,text="Picture, Image,Photo file",bg="#2f4155",fg="yellow").place(x=20,y=5)

        window.mainloop()    
        #========================================================================================
        #========================================================================================       
        

        #===========================================================================================
        #===========================================================================================
        

    else:
        messagebox.showerror('Invalid','invalid username or password')

    

img = PhotoImage(file='login1.png')
Label(root,image=img,bg='#08082b').place(x=50,y=70)


frame=Frame(root,width=350,height=350,bg='#08082b')
frame.place(x=480,y=70)

original_image = Image.open('login_logo.png')

# Resize the image to the desired size
desired_size = (60, 60)  # Adjust the size as needed
resized_image = original_image.resize(desired_size, Image.LANCZOS)

# Convert the resized image to PhotoImage
img1 = ImageTk.PhotoImage(resized_image)
Label(frame,image=img1,bg='#08082b').place(x=150,y=-2)
heading=Label(frame,text='Sign in',fg='#57a1f8',bg='#08082b',font=('Microsoft YaHei UI Light',11,'bold'))
heading.place(x=150,y=58)


#-----------------------------------------USERNAME--------------------------------------------------------

def on_enter(e):
    user.delete(0,'end')

def on_leave(e):
    name=user.get()
    if name=='':
        user.insert(0,'Example')

user = Entry(frame,width=25,fg='white',border=0,bg="#08082b",font=('Microsoft YaHei UI Light',11))
user.place(x=90,y=100)
username_Label = Label(frame,text="Username: ",fg='white',border=0,bg="#08082b",font=('Microsoft YaHei UI Light',12,'bold'))
username_Label.place(x=-3,y=100)

user.insert(0,'Example')
user.bind('<FocusIn>',on_enter)
user.bind('<FocusOut>', on_leave)

Frame(frame,width=295,height=2,bg='white').place(x=90,y=125)




#-------------------------------------------PASSWORD------------------------------------------------------

def on_enter(e):
    code.delete(0,'end')
    
def on_leave(e):
    name=user.get()
    if name=='':
       code.insert(0,'Password')
        
code = Entry(frame,width=25,fg='white',border=0,bg="#08082b",font=('Microsoft YaHei UI Light',11))
code.config(show='*')
code.place(x=95,y=158)
username_Label = Label(frame,text="Password: ",fg='white',border=0,bg="#08082b",font=('Microsoft YaHei UI Light',12,'bold'))
username_Label.place(x=-3,y=158)

code.insert(0,'Password')
code.bind('<FocusIn>',on_enter)
code.bind('<FocusOut>', on_leave)


Frame(frame,width=295,height=2,bg='white').place(x=90,y=177)

#------------------------------------------------------------------------------------------------

Button(frame,width=39,pady=7,text='sign in',bg='white',fg='black',border=0,command=signin).place(x=35,y=220)
label=Label(frame,text="ONLY FOR OFFICIAL USE AND ADMIN LOGIN",fg='white',bg='#08082b',font=('Microsoft YaHei UI Light',9))
label.place(x=55,y=290)


root.mainloop()

