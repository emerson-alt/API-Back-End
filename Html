import mysql.connector
from fastapi import FastAPI, Request, Form, WebSocket
from fastapi.responses import HTMLResponse, RedirectResponse
from fastapi.templating import Jinja2Templates
from datetime import datetime

app = FastAPI(title="API Restaurante Popular")
templates = Jinja2Templates(directory="templates")

# Lista de clientes conectados pelo WebSocket
connected_clients = []

def get_connection():
    return mysql.connector.connect(
        host='localhost',
        user='root',
        password='1704',
        database='restaurante_popular'
    )

# ---------------------------------------------------------
# ROTA WEBSOCKET – broadcast simples
# ---------------------------------------------------------
@app.websocket("/ws")
async def websocket_endpoint(websocket: WebSocket):
    await websocket.accept()
    connected_clients.append(websocket)

    try:
        while True:
            await websocket.receive_text()  # mantem conexão viva
    except:
        connected_clients.remove(websocket)

# ---------------------------------------------------------
#  PÁGINA PRINCIPAL
# ---------------------------------------------------------
@app.get("/", response_class=HTMLResponse)
def home(request: Request, msg: str = ""):
    try:
        conn = get_connection()
        cursor = conn.cursor(dictionary=True)
        cursor.execute("SELECT * FROM cliente")
        clientes = cursor.fetchall()
        cursor.execute("SELECT * FROM plano")
        planos = cursor.fetchall()
        cursor.close()
        conn.close()
    except Exception as e:
        return HTMLResponse(f"<h1>Erro ao conectar com o banco:</h1><p>{e}</p>")

    return templates.TemplateResponse("index.html", {
        "request": request, "clientes": clientes, "planos": planos, "msg": msg
    })

# ---------------------------------------------------------
# Cadastrar cliente + pagamento + plano + visita
#    E disparar WebSocket para atualizar dashboard
# ---------------------------------------------------------
@app.post("/add_cliente")
async def add_cliente(
    nome: str = Form(...),
    cpf: str = Form(...),
    data_nascimento: str = Form(...),
    plano_id: int = Form(...),
    valor_pago: float = Form(...),
    data_visita: str = Form(...)
):
    try:
        conn = get_connection()
        cursor = conn.cursor()

        # Cliente
        cursor.execute(
            "INSERT INTO cliente (nome, cpf, data_nascimento) VALUES (%s, %s, %s)",
            (nome, cpf, data_nascimento)
        )
        cliente_id = cursor.lastrowid

        # Pagamento
        cursor.execute(
            "INSERT INTO pagamento (cliente_id, valor, data_pagamento) VALUES (%s, %s, %s)",
            (cliente_id, valor_pago, datetime.now())
        )

        # Plano
        cursor.execute(
            "INSERT INTO cliente_plano (cliente_id, plano_id) VALUES (%s, %s)",
            (cliente_id, plano_id)
        )

        # Visita
        cursor.execute(
            "INSERT INTO visita (cliente_id, data_visita) VALUES (%s, %s)",
            (cliente_id, data_visita)
        )

        conn.commit()
        cursor.close()
        conn.close()

        # -------------------------------
        # Enviar mensagem via WebSocket
        # -------------------------------
        for client in connected_clients:
            await client.send_text("UPDATE")

        msg = "Cliente cadastrado com sucesso!"

    except Exception as e:
        msg = f"Erro ao cadastrar cliente: {e}"

    return RedirectResponse(f"/?msg={msg}", status_code=303)
