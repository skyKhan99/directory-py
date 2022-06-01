# directory-py
A program for save people's name, surname and age like directory. Written by Python.

# libraries
tkinter, json, os

# class person
class person:

    def __init__(self, name, surname, age):
        self.name = name
        self.surname = surname
        self.age = age
   
# create new person method
def new_person():

    new_tk = tk.Tk()
    new_tk.resizable(False, False)
    new_tk.title('New')
    new_tk.configure(background='lightgreen')
    name_label = tk.Label(new_tk, text='Name:', font=('Calibri', 20), background='lightgreen', foreground='darkgreen')
    surname_label = tk.Label(new_tk, text='Surname:', font=('Calibri', 20), background='lightgreen', foreground='darkgreen')
    age_label = tk.Label(new_tk, text='Age:', font=('Calibri', 20), background='lightgreen', foreground='darkgreen')

    name_entry = tk.Entry(new_tk, font=('Calibri', 20), foreground='lightgreen', background='darkgreen')
    surname_entry = tk.Entry(new_tk, font=('Calibri', 20), foreground='lightgreen', background='darkgreen')
    age_entry = tk.Entry(new_tk, font=('Calibri', 20), foreground='lightgreen', background='darkgreen')

    name_label.grid(row=0, column=0, sticky='e')
    surname_label.grid(row=1, column=0, sticky='e')
    age_label.grid(row=2, column=0, sticky='e')

    name_entry.grid(row=0, column=1)
    surname_entry.grid(row=1, column=1)
    age_entry.grid(row=2, column=1)

    def save():
        p = person(name_entry.get(), surname_entry.get(), age_entry.get())
        infos = []
        with open("jsons/count.txt", "r") as read:
            count = int(read.read())
        info_json = {f"{count}": {'id': count,'name': p.name, 'surname': p.surname, 'age': p.age}}
        with open(f"jsons/info_save{count}.json", "a") as write_file:
            json.dump(info_json, write_file)
        with open("jsons/count.txt", "w") as write:
            write.write(str(count+1))
        infos.append((count, p.name, p.surname, p.age))
        for info in infos:
            tree.insert('', tk.END, values=info)
        new_tk.destroy()

    save_btn = tk.Button(new_tk, text='Save', command=save, font=('Calibri', 20), background='lightyellow')
    save_btn.grid(row=3, column=1, sticky='we')
    
# delete person method
def delete_person():

    if messagebox.askyesno("Delete?", "Are you sure want to delete?"):
        delete_wanted_person = tree.item(tree.selection())["values"][0]
        os.remove(f"jsons/info_save{int(delete_wanted_person)}.json")
        tree.delete(tree.selection())
        
# edit person method
def edit_person():

    edit_wanted_person = tree.item(tree.selection())["values"][0]
    name = tree.item(tree.selection())["values"][1]
    surname = tree.item(tree.selection())["values"][2]
    age = tree.item(tree.selection())["values"][3]
    edit_tk = tk.Tk()
    edit_tk.resizable(False, False)
    edit_tk.title('Edit')
    edit_tk.configure(background='lightblue')
    name_label = tk.Label(edit_tk, text='Name:', font=('Calibri', 20), background='lightblue', foreground='darkblue')
    surname_label = tk.Label(edit_tk, text='Surname:', font=('Calibri', 20), background='lightblue', foreground='darkblue')
    age_label = tk.Label(edit_tk, text='Age:', font=('Calibri', 20), background='lightblue', foreground='darkblue')

    name_entry = tk.Entry(edit_tk, font=('Calibri', 20), foreground='lightblue', background='darkblue')
    surname_entry = tk.Entry(edit_tk, font=('Calibri', 20), foreground='lightblue', background='darkblue')
    age_entry = tk.Entry(edit_tk, font=('Calibri', 20), foreground='lightblue', background='darkblue')

    name_entry.insert(0, name)
    surname_entry.insert(0, surname)
    age_entry.insert(0, age)

    name_label.grid(row=0, column=0, sticky='e')
    surname_label.grid(row=1, column=0, sticky='e')
    age_label.grid(row=2, column=0, sticky='e')

    name_entry.grid(row=0, column=1)
    surname_entry.grid(row=1, column=1)
    age_entry.grid(row=2, column=1)


    def save():
        p = person(name_entry.get(), surname_entry.get(), age_entry.get())
        infos = []
        with open(f"jsons/info_save{edit_wanted_person}.json", "w") as w:
            info_json = {f"{edit_wanted_person}": {'id': edit_wanted_person, 'name': p.name, 'surname': p.surname, 'age': p.age}}
            json.dump(info_json, w)
        infos.append((count, p.name, p.surname, p.age))
        tree.delete(tree.selection())
        for info in infos:
            tree.insert('', tk.END, values=info)
        edit_tk.destroy()


    save_btn = tk.Button(edit_tk, text='Save', command=save, font=('Calibri', 20), background='lightyellow')
    save_btn.grid(row=3, column=1, sticky='we')

# Creating main screen
codes:

  > root = tk.Tk()
  > root.title("Directory")
  > root.geometry('625x350')
  > root.resizable(False, False)

  > tree = ttk.Treeview(root, columns=('id', 'name', 'surname', 'age'), show='headings')
  > tree.heading('id', text='ID')
  > tree.heading('name', text='Name')
  > tree.heading('surname', text='Surname')
  > tree.heading('age', text='Age')
  > tree.column(0, width=5)

  > scrollbar = ttk.Scrollbar(root, orient=tk.VERTICAL, command=tree.yview)
  > tree.configure(yscrollcommand=scrollbar.set)
  > scrollbar.grid(row=0, column=1, sticky='ns')
  > tree.grid(row=0, column=0, sticky='ew')

  > new_person_btn = tk.Button(text='New', command=new_person, font=('Calibri', 15),bg='lightgreen')
  > new_person_btn.grid(row=1, column=0, sticky='we')
  > delete_person_btn = tk.Button(text='Delete', command=delete_person, font=('Calibri', 15), bg='lightpink')
  > delete_person_btn.grid(row=2, column=0, sticky='ew')
  > edit_person_btn = tk.Button(text='Edit', command=edit_person, font=('Calibri', 15), bg='lightblue')
  > edit_person_btn.grid(row=3, column=0, sticky='ew')

> try:
    > with open("jsons/count.txt", "r") as read:
        > count = int(read.read())
    > for i in range(count):
        > try:
            > with open(f"jsons/info_save{i}.json", 'r') as read_file:
                > saves = json.loads(read_file.read())
                > for i in saves:
                    > infos = []
                    > infos.append((saves[i]['id'], saves[i]['name'], saves[i]['surname'], saves[i]['age']))
                    > for info in infos:
                        > tree.insert('', tk.END, values=info)
        > except:
            > pass
> except:
    > pass
> root.mainloop()
