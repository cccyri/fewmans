import sys
import os
import csv
from PyQt5.QtWidgets import QTableWidget, QApplication, QMainWindow, QTableWidgetItem, QFileDialog
from PyQt5.QtWidgets import qApp, QAction
import sys
import os
import csv
import pandas as pd
from PyQt5.QtWidgets import QTableWidget, QApplication, QMainWindow, QTableWidgetItem, QFileDialog,QWidget



class MyTable(QTableWidget):
    def __init__(self, r, c):
        super().__init__(r, c)
        self.check_change = True
        self.init_ui()

    def init_ui(self):
        self.cellChanged.connect(self.c_current)
        self.show()

    def c_current(self):
        if self.check_change:
            row = self.currentRow()
            col = self.currentColumn()
            value = self.item(row, col)
            value = value.text()
            print("The current cell is ", row, ", ", col)
            print("In this cell we have: ", value)

    def open_sheet(self):
        self.check_change = False
        path = QFileDialog.getOpenFileName(self, 'Open CSV', os.getenv('HOME'), 'CSV(*.csv)')
        if path[0] != '':
            with open(path[0], newline='') as csv_file:
                self.setRowCount(0)
                self.setColumnCount(10)
                # my_file = csv.reader(csv_file, delimiter=';', quotechar='|')
                df = pd.read_csv(csv_file, delimiter=';', quotechar='|')
                '''for row_data in my_file:
                    row = self.rowCount()
                    self.insertRow(row)
                    if len(row_data) > 10:
                        self.setColumnCount(len(row_data))
                    for column, stuff in enumerate(row_data):
                        item = QTableWidgetItem(stuff)
                        self.setItem(row, column, item)'''
                # df = pd.read_csv(my_file, delimiter=';')

                df.columns = ['id', 'gender', 'hair', 'eyes', 'body', 'sexuality', 'intelligence', 'career', 'curse',
                              "god's gift"]

                groups = df.groupby(df.gender)
                df1 = groups.get_group("female")
                df2 = groups.get_group("male")

                df4 = pd.DataFrame()
                ind = 1

                for index1, row1 in df1.iterrows():
                    for index2, row2 in df2.iterrows():
                        rate = 0
                        if (row1["hair"] == row2["hair"]):
                            rate += 1
                        if (row1["gender"] == row2["gender"]):
                            rate += 1
                        if (row1["body"] == row2["body"]):
                            rate += 1
                        if (row1["eyes"] == row2["eyes"]):
                            rate += 1
                        if (row1["sexuality"] == row2["sexuality"]):
                            rate += 1
                        if (row1["intelligence"] == row2["intelligence"]):
                            rate += 1
                        if (row1["career"] == row2["career"]):
                            rate += 1
                        if (row1["curse"] == row2["curse"]):
                            rate += 1
                        if (row1["god's gift"] == row2["god's gift"]):
                            rate += 1

                        df228 = pd.DataFrame([[rate, row1["id"], row2["id"]]],
                                             columns=['rating', 'mother_id', 'father_id'],
                                             index=[10000 + ind])

                        ind = ind + 1
                        df4 = df4.append(df228)

                df4 = df4.sort_values(by='rating', ascending=False)

                if (df1.shape[0] < df2.shape[0]):
                    df_new = pd.DataFrame()
                    ind = 1
                    while (len(df4.mother_id) != 0):
                        df5 = df4.groupby(df4.mother_id)
                        print(df5.get_group((list(df5.groups)[0])), "\n\n")
                        df_temp1 = pd.DataFrame()
                        df_temp1 = df5.get_group(list(df5.groups)[0])
                        df_temp = pd.DataFrame(
                            [[df_temp1.iloc[0]["rating"], df_temp1.iloc[0]["mother_id"],
                              df_temp1.iloc[0]["father_id"]]],
                            columns=['best_rating', 'mother_id', 'father_id'],
                            index=[20000 + ind])
                        df4 = df4.drop(df4[df4.mother_id == df_temp1.iloc[0]["mother_id"]].index)
                        df4 = df4.drop(df4[df4.father_id == df_temp1.iloc[0]["father_id"]].index)
                        ind = ind + 1
                        df_new = df_new.append(df_temp)
                    df_no_pair = df4 #те кто без пары остались

                    with open('ans.csv', 'w') as csv_file:
                        df_new.to_csv(csv_file, sep=';', header="", index = 'False', line_terminator='\n')
                    print(df_new)
                else:
                    df_new = pd.DataFrame()
                    ind = 1
                    while (len(df4.father_id) != 0):
                        df5 = df4.groupby(df4.father_id)
                        print(df5.get_group((list(df5.groups)[0])), "\n\n")
                        df_temp1 = pd.DataFrame()
                        df_temp1 = df5.get_group(list(df5.groups)[0])
                        df_temp = pd.DataFrame(
                            [[df_temp1.iloc[0]["rating"], df_temp1.iloc[0]["father_id"],
                              df_temp1.iloc[0]["mother_id"]]],
                            columns=['best_rating', 'father_id', 'mother_id'],
                            index=[20000 + ind])
                        df4 = df4.drop(df4[df4.mother_id == df_temp1.iloc[0]["mother_id"]].index)
                        df4 = df4.drop(df4[df4.father_id == df_temp1.iloc[0]["father_id"]].index)
                        ind = ind + 1
                        df_new = df_new.append(df_temp)
                    df_no_pair = df4 #те кто без пары остались

                    with open('ans.csv', 'w') as csv_file:
                        df_new.to_csv(csv_file, sep=';', header="", index = 'False', line_terminator='\n')
                    print(df_new)

        self.check_change = True

    def save_sheet(self):
        path = QFileDialog.getSaveFileName(self, 'Save CSV', os.getenv('HOME'), 'CSV(*.csv)')
        if path[0] != '':
            with open(path[0], 'w') as csv_file:
                writer = csv.writer(csv_file, dialect='excel')
                for row in range(self.rowCount()):
                    row_data = []
                    for column in range(self.columnCount()):
                        item = self.item(row, column)
                        if item is not None:
                            row_data.append(item.text())
                        else:
                            row_data.append('')
                    writer.writerow(row_data)


class Sheet(QMainWindow):
    def __init__(self):
        super().__init__()

        self.form_widget = MyTable(10, 10)
        self.setCentralWidget(self.form_widget)
        col_headers = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J']
        self.form_widget.setHorizontalHeaderLabels(col_headers)

        # Set up menu
        bar = self.menuBar()
        file = bar.addMenu('File')

        save_action = QAction('&Save', self)
        save_action.setShortcut('Ctrl+S')

        open_action = QAction('&Open', self)

        quit_action = QAction('&Quit', self)

        file.addAction(save_action)
        file.addAction(open_action)
        file.addAction(quit_action)

        quit_action.triggered.connect(self.quit_app)
        save_action.triggered.connect(self.form_widget.save_sheet)
        open_action.triggered.connect(self.form_widget.open_sheet)
        self.show()

    def quit_app(self):
        qApp.quit()

app = QApplication(sys.argv)
sheet = Sheet()
sys.exit(app.exec_())
