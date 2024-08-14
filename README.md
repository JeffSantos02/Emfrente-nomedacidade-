# EMFRENTE(NOME DA CIDADE)
TESTE DE SITE ONDE SERAM FEITAS OCORRÊNCIAS DE IRREGULARIDADES NOS BAIRROS. DANDO ASSIM AGILIDADE NOS SERVIÇOS PRESTADOS POR CONSECIONÁRIAS RESPONSÁVEIS.

import tkinter as tk
from tkinter import messagebox
import json
import os

# Arquivo de dados
USERS_FILE = 'users.json'
REPORTS_FILE = 'reports.json'

# Funções para manipulação de arquivos
def load_data(file_name):
    if not os.path.exists(file_name):
        return {}
    with open(file_name, 'r') as file:
        return json.load(file)

def save_data(file_name, data):
    with open(file_name, 'w') as file:
        json.dump(data, file, indent=4)

# Funções para tela de login
def login():
    username = entry_username.get()
    password = entry_password.get()
    
    users = load_data(USERS_FILE)
    
    if username in users and users[username] == password:
        login_window.destroy()
        main_window()
    else:
        messagebox.showerror('Erro', 'Nome de usuário ou senha inválidos')

def open_signup_window():
    login_window.destroy()
    signup_window()

# Funções para tela de cadastro
def signup():
    username = entry_signup_username.get()
    password = entry_signup_password.get()
    
    users = load_data(USERS_FILE)
    
    if username in users:
        messagebox.showerror('Erro', 'Nome de usuário já existe')
    else:
        users[username] = password
        save_data(USERS_FILE, users)
        messagebox.showinfo('Sucesso', 'Cadastro realizado com sucesso!')
        signup_window.destroy()
        login_window()

def signup_window():
    global entry_signup_username, entry_signup_password
    
    window = tk.Tk()
    window.title('Cadastro de Novo Usuário')

    tk.Label(window, text='Nome de Usuário').pack()
    entry_signup_username = tk.Entry(window)
    entry_signup_username.pack()

    tk.Label(window, text='Senha').pack()
    entry_signup_password = tk.Entry(window, show='*')
    entry_signup_password.pack()

    tk.Button(window, text='Cadastrar', command=signup).pack()
    tk.Button(window, text='Voltar', command=lambda: [window.destroy(), open_login_window()]).pack()

    window.mainloop()

# Funções para tela principal
def main_window():
    window = tk.Tk()
    window.title('Tela Principal')

    def report_event():
        event = entry_event.get()
        location = entry_location.get()
        reports = load_data(REPORTS_FILE)
        
        if location not in reports:
            reports[location] = []
        reports[location].append(event)
        save_data(REPORTS_FILE, reports)
        messagebox.showinfo('Sucesso', 'Boletim registrado com sucesso!')

    tk.Label(window, text='Registro de Ocorrências').pack()

    tk.Label(window, text='Descrição do Evento').pack()
    entry_event = tk.Entry(window)
    entry_event.pack()

    tk.Label(window, text='Localização').pack()
    entry_location = tk.Entry(window)
    entry_location.pack()

    tk.Button(window, text='Registrar Ocorrência', command=report_event).pack()

    tk.Button(window, text='Sair', command=window.quit).pack()

    window.mainloop()

# Funções para tela de login
def open_login_window():
    global entry_username, entry_password
    
    global login_window
    login_window = tk.Tk()
    login_window.title('Tela de Login')

    tk.Label(login_window, text='Nome de Usuário').pack()
    entry_username = tk.Entry(login_window)
    entry_username.pack()

    tk.Label(login_window, text='Senha').pack()
    entry_password = tk.Entry(login_window, show='*')
    entry_password.pack()

    tk.Button(login_window, text='Login', command=login).pack()
    tk.Button(login_window, text='Cadastrar Novo Usuário', command=open_signup_window).pack()

    login_window.mainloop()

if __name__ == "__main__":
    open_login_window()
