# app-para-agendar-hora-de-desligamento-do-PC
import tkinter as tk
from tkinter import messagebox
import time
import os
import threading


def shutdown_at_time(target_time):
    while True:
        current_time = time.strftime("%H:%M")
        if current_time == target_time:
            os.system("shutdown /s /t 1")
            break
        time.sleep(30)  # Verifica a cada 30 segundos


def start_shutdown():
    target_time = entry.get()
    if not target_time:
        messagebox.showwarning("Entrada inválida", "Por favor, insira uma hora válida no formato HH:MM")
        return

    try:
        time.strptime(target_time, "%H:%M")
    except ValueError:
        messagebox.showwarning("Formato de hora inválido", "Por favor, insira a hora no formato HH:MM")
        return

    messagebox.showinfo("Agendado", f"Computador será desligado às {target_time}")
    threading.Thread(target=shutdown_at_time, args=(target_time,), daemon=True).start()


# Configuração da interface gráfica
root = tk.Tk()
root.title("Agendar Desligamento")

label = tk.Label(root, text="Insira a hora para desligar (HH:MM):")
label.pack(pady=10)

entry = tk.Entry(root)
entry.pack(pady=5)

button = tk.Button(root, text="Agendar Desligamento", command=start_shutdown)
button.pack(pady=20)

root.mainloop()
