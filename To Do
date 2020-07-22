from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy import Column, Integer, String, Date
from datetime import datetime
import datetime as dt
from sqlalchemy.orm import sessionmaker
import sys

engine = create_engine('sqlite:///todo.db?check_same_thread=False')

Base = declarative_base()


class Table(Base):
    __tablename__ = 'task'
    id = Column(Integer, primary_key=True)
    task = Column(String, default='default_value')
    deadline = Column(Date, default=datetime.today().date())

    def __repr__(self):
        return self.string_field


Base.metadata.create_all(engine)

Session = sessionmaker(bind=engine)
session = Session()


def display_all():
    rows = session.query(Table).all()

    if rows:
        for i in range(len(rows)):
            deadline = rows[i].deadline
            print(f"{i + 1}. {rows[i].task}. {deadline.strftime('%d %b')}")
    else:
        print("Nothing to do!")


def display_today():
    rows = session.query(Table).filter(Table.deadline == datetime.today().date()).all()
    if rows:
        print(datetime.today().date().strftime("%d %b"))
        for i in range(len(rows)):
            print(f"{i + 1}. {rows[i].task}")
    else:
        print("Nothing to do!")

weekdays = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]

def display_week():
    for i in range(7):
        rows = session.query(Table).filter(Table.deadline == (datetime.today().date() + dt.timedelta(days = i))).all()
        print(f"{weekdays[(datetime.today() + dt.timedelta(days = i)).weekday()]} {(datetime.today() + dt.timedelta(days = i)).strftime('%d %b')}")
        if rows:
            for j in range(len(rows)):
                print(f"{j + 1}. {rows[j].task}")
            print()
        else:
            print("Nothing to do!")
            print()

def add_to_list():
    print("Enter task:")
    task = input()
    print("Enter deadline")
    deadline = datetime.strptime(input(), '%Y-%m-%d')
    new_row = Table(task=task, deadline=deadline)
    session.add(new_row)
    session.commit()
    print("Task has been added")


def missed_tasks():
    rows = session.query(Table).filter(Table.deadline < datetime.today().date()).all()
    print("Missed tasks:")
    if rows:
        for i in range(len(rows)):
            print(f"{i + 1}. {rows[i].task}. {rows[i].deadline.strftime('%d %b')}")
    else:
        print("Nothing is missed!")


def delete_tasks():
    rows = session.query(Table).order_by(Table.deadline).all()
    if not rows:
        print("Nothing to delete!")
    else:
        print("Chose the number of the task you want to delete:")
        for i in range(len(rows)):
            print(f"{i+1}. {rows[i].task}. {rows[i].deadline.strftime('%d %b')}")
    specific_row = rows[int(input())-1]
    session.delete(specific_row)
    session.commit()
    print("The task has been deleted!")


def to_do_list():
    if option in ['1', "First Task", "First task"]:
        display_today()
        print()
    if option in ['2', "Second Task", "Second task"]:
        display_week()
        print()
    if option in ['3', "Third Task", "Third task"]:
        display_all()
        print()
    if option in ['4', "Fourth Task", "Fourth task"]:
        missed_tasks()
        print()
    if option in ['5', "Five Task", "Five task"]:
        add_to_list()
        print()
    if option in ['6', "Six Task", "Six task"]:
        delete_tasks()
        print()

print("""1) Today's tasks
2) Week's tasks
3) All tasks
4) Missed tasks
5) Add task
6) Delete task
0) Exit""")

option = input()

while not(option == '0'):
    to_do_list()
    print("""1) Today's tasks
2) Week's tasks
3) All tasks
4) Missed tasks
5) Add task
6) Delete task
0) Exit""")
    option = input()
else:
    print("Bye")
    sys.exit()
