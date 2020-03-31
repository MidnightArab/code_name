# store phone numbers and email addresses in this Python script
import pickle

# create a list of names in the directory
def list_(dict):
    return list(dict.keys())

#check to make sure input for number value is integer    
def num_input():
  try:
    a = int(input('Number: '))
    return a
  except ValueError:
    print('Please enter numbers only.')
    return num_input()
    
def addnum():
    #load contents of pickle file into dictionary
    dict = pickle.load(open('directory.pkl','rb'))

    #create an object and assign three values to it
    #name,number,email
    class Item():
        def __init__(self,name,number,email):
            self.name = name
            self.number = number
            self.email = email
    b = input('name: ')
    if b in dict:
        print(b,'is already in the directory.')
        return
    c = num_input()
    e = input('email: ')
    if e == '':
        e = 'None'
    b = Item(b,c,e)

    #assign the three values to a variable
    var1 = vars(b)
    #assign the name value to a second variable
    var2 = var1['name']
    #assign the three values of the first variable to a dictionary with the second variable as key
    dict[var2]= var1
    #delete the original object
    del b
    #dump modified dictionary back into pickle file
    pickle.dump(dict,open('directory.pkl','wb'))

#makes phone numbers easier to read, works best if number is nine digits long
def phone_format(n):
    h = ""
    for i in range(len(n)):
        h += n[i]
        if len(h) <= 9:
            if i % 3 == 2:
                if i % 8 != 0:
                    h += '-'
    return h

#a semi autocomplete function. Enter name one letter at a time 
def auto_cmplet():
    d = pickle.load(open('directory.pkl','rb'))
    #create a list containing all names in directory for easier search
    c = list_(d)
    for i in d:
      c.append(str(d[i]['number']))
    for o in range(10): 
        z = []     
        if o == 0:
            s = input('Enter the first letter of the name you\'re looking for. Enter "no" at any time to quit: ')
            if s == 'no':
              break
        if o > 0:
            print('Enter the next letter:',s)
            mn = input()
            s += mn
            if mn == 'no':
              break
        for i in c:
            if i.startswith(s):
                print(i,phone_format(str(d[i]['number'])))
                z.append(i)
        if len(z) == 0:
                print('No name found')
                return

#use this to search for a specific name
def findnum():
    d = pickle.load(open('directory.pkl','rb'))
    f = input('Enter the person\'s name whose number you need. To see a list of all names in directory enter all:\n')
    list_(d)
    
    if f == "all":
        print('There are',len(d),'numbers in directory.')
        a = []
        for i in d:
            a.append(d[i]['name'])
        a.sort()
        for i in range(len(a)):
            print("Name:",a[i],'\nNumber:',d[a[i]]['number'],'\n')
        x = input()
        
    elif f not in d:
        print(f,'is not in directory')
        
    else:
        v = str(d[f]['number'])
        print('Number:',phone_format(v))
        print('Email: ',d[f]['email'])


def edit_name():
    d = pickle.load(open('directory.pkl','rb'))
    c = list_(d)
    name = input('Enter the name you wish to edit: ')
    if name in d:
        b = input('name: ')
        if b == '':
            b = name
        c = num_input()
        e = input('email: ')
        if e == '':
            e = 'None'
        del d[name]
        bc = {b:{'name':b,'number':c,'email':e}}
        z = b
        d[z] = bc[b]
        pickle.dump(d,open('directory.pkl','wb'))
        print('Item has been successfully changed.')
    else:
        print(name,'is not in directory.')


def del_name():
    d = pickle.load(open('directory.pkl','rb'))
    name = input('Enter the name you wish to delete: ')
    if name in d:
        act = input('enter d to confirm: ')
        if act == 'd':
            del d[name]
            pickle.dump(d,open('directory.pkl','wb'))
            print(name,'has been successfully deleted.')
    else:
        print(name,'is not in directory.')


def run_direct():
    while True:
        l = input('\nHome Menu \nType these commands \n"a" to enter a new number \n"f" to get a stored number \n"e" to edit an existing number  \n"d" delete a number\n"ac" for autocomplete\n')
        if l == 'a':
            addnum()
        elif l == 'f':
            findnum()
        elif l == 'e':
            edit_name()
        elif l == 'd':
            del_name()
        elif l == 'ac':
            auto_cmplet()
        else:
          print('Unknown input. Please try again.')
#if __name__ == '__main__':

run_direct()
