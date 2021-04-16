from collections import namedtuple # нужно для использование кортежей
file=open('decan.txt','w') # создаем файл
s1=int(input("Введите кол-во студентов: "))
k1=int(input("Введите кол-во предметов: "))
s=[]*s1
k=[]*k1
Jour=[]#тут будет список всех студентов с предметов и оценкой
wh=1
zz4=0 #Для 4 задания
zz5=0 #6 задание, для подсчета кол-во 4
zzz5=0#6 задание, для подсчета кол-во 4
kk1=0#для задания 5
nnp=[]#список неуспевающих студентов задание 6
pp2=0 #кол-во хорошистов студентов для 5 задания
pp1=0 #Для задания 5
pp=0 #По скольким предметам выставили оценки-нужно для 4 задания
sz=0 #для 2 задания-складываем оценки ученика по всем предметам
sz1=0 #для 2 задания-узнаем колв оценок
ssz=[] #Массив для 2 задания, чтобы записать кортеж
szp=0 #для 3 задания-складываем оценки группы по предмету
szp1=0 #для 3 задания-складываем кол-ов оценок группы
ssp=[] #Массив для 3 задания, чтобы записать кортеж
sf=[] #Для задания 4, тут будут фамилии
Journal=namedtuple('Journal', ['FIO',  'predmet', 'ocenka']) #используем кортеж
SO=namedtuple('SO',['FIO1','srbl']) #для 2 задания 1)ФИО, 2)Среднее значение, так же используем кортеж
SOP=namedtuple('SOP',['predmet1','sbl']) #для 3 задания 1)Предмет 3)Сренее значение
for i in range(s1):
    s2=str(input("Введите ФИО студента: ")) #Вводим ФИО студентов
    s.append(s2)
for i in range (k1):
    k2=str(input("Введите название предмета: ")) #Вводим название предмета
    k.append(k2)
def ocenk(v): #функция для выставлени оценок
        for i in range(s1):
            print("Введите оценку",s[i],"по ", k[v]) #выводим именна студентов и номер предмета который мы выбрали
            st=Journal(s[i],k[v],int(input("Введите оценку: ")))#записываем в кортеж ФИО, назвение предмета, оценку
            Jour.append(st) # вносим кортеж в массив
def zad1():
     file.write("Список группы с оценками: \n")
     for i in range(len(Jour)):
          print(Jour[i].FIO,"Оценка по предмету ",Jour[i].predmet,":", Jour[i].ocenka)  #выводим список группы
          file.write((Jour[i].FIO+" оценка по предмету "+Jour[i].predmet+":"+str(Jour[i].ocenka))+'\n') #записываем в файл
def zad2():
    global sz, sz1
    for i in range (s1): #цикл для учеников; берем ученика и складываем оценки
             for j in range (len(Jour)):
                if Jour[i].FIO==Jour[j].FIO: #проверка, чтобы имя ученика было такое же как в кортеже
                    sz+=Jour[j].ocenka
                    sz1+=1
             sz=sz/sz1
             st=SO(Jour[i].FIO,sz)
             ssz.append(st)
             sz=0
             sz1=0
    file.write("Средний бал каждого студента \n")
    for i in range (len(ssz)):
              print(ssz[i].FIO1, ssz[i].srbl)
              file.write((ssz[i].FIO1+ str(ssz[i].srbl))+'\n')
def zad3():
    global szp, szp1
    for i in range (k1): # берем предметы
        for j in range (len(Jour)):
            if k[i]==Jour[j].predmet: #проверка, чтоыб предметы с кортежа ровнялись предметами с массива, как в функции zad2
                szp+=Jour[j].ocenka
                szp1+=1
        szp=szp/szp1
        st=SOP(k[i],szp)
        ssp.append(st)
        szp=0
        szp1=0
    file.write("Средний бал группы по предмету: \n")
    for i in range (len(ssp)):
        print(ssp[i].predmet1,ssp[i].sbl)
        file.write((ssp[i].predmet1+str(ssp[i].sbl))+"\n")
def zad4():
    global zz4
    for i in range(s1):
            for j in range(len(Jour)):
                if s[i]==Jour[j].FIO:
                    if Jour[j].ocenka==5: # смотрим, чтобы кол-во 5 равнялось количеству предметов по которым стоят оценки
                        zz4+=1
            if zz4==pp:
                sf.append(s[i])
            zz4=0
    print(sf)
    file.write("Списки фамилий,назначенных на повышенную стпендию: \n")
    file.write(str(sf)+'\n')
def zad5():
     global zz5, zzz5, pp2
     for i in range (s1):
            for j in range(len(Jour)):
                if s[i]==Jour[j].FIO:
                    if Jour[j].ocenka==4:
                        zz5+=1  
                    else: 
                        if Jour[j].ocenka==5:
                          zzz5+=1
            if zz5>0 and zzz5+zz5==pp: # тут смотрим чтообы стояла хотя бы одна 4 и не было 3
                pp2+=1
            zz5=0
            zzz5=0
     print("Кол-во студентов назначенных на обычную степендию: ", pp2)
     file.write(("Кол-во студентов назначенных на обычную степендию: "+ str(pp2))+"\n")
def zad6():
    global kk1
    for i in range (s1):
             for j in range (len(Jour)):
                 if s[i]==Jour[j].FIO:
                     if Jour[j].ocenka==2:
                         kk1+=1
             if kk1>0: # проверка, чтобы была 1 двойка
                 nnp.append(s[i])
             kk1=0
    print(nnp)
    file.write("список неуспевающих студентов: \n")
    file.write(str(nnp))  
while wh!=0:
    oc=int(input("Введите любое число кроме 0, хотите выставить оценки: "))
    while oc!=0 and pp!=k1:
        for i in range(k1):
            print (k[i])
        nm=int(input("Введите номер предмета, по которому хотите ввести оценку: "))
        x=ocenk(nm-1)
        pp+=1
        oc=int(input("Если больше не будете вводить оценки, нажмите 0 "))
    print ("PP=",pp)
    z1=int(input("Введите 1, если хотите увидеть весь список группы с оценками или любое число, чтобы пропустить этот пункт: "))
    if z1==1:
        zad1()
    z2=int(input("Введите 1 если хотите узнать средний балл каждого студента или любое число, чтобы пропустить этот пункт: "))
    if z2==1:
            zad2()
    z3=int(input("Введите 1 если хотите узнать средний бал группы по предмету или любое число, чтобы пропустить этот пункт: "))
    if z3==1:
       zad3()
    z4=int(input("Введите 1, чтобы получить списки фамилий назначеных на повышенную степендию или любое число, чтобы пропустить этот пункт: "))
    if z4==1:
        zad4()
    z5=int(input("Введите 1, чтобы получить списки фамилий назначеных на обычную степендию или любое число, чтобы пропустить этот пункт: "))
    if z5==1:
        zad5()
    z6=int(input("Введите 1,чтобы вывести список неуспевающих студентов или любое число, чтобы пропустить этот пункт:"))
    if z6==1:
        zad6()
    wh=int(input("Напишите 0 для завершения программы или другое число для продолжения работы: "))
file.close()
        

