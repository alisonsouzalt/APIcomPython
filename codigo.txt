# API - É um lugar para disponibilizar recursos e/ou funcionalidades
# 1. Objetivo - Criar um ap que disponibiliza a consulta, criação, edição e exclusão de livros.
# 2. URL base - localhost
# 3. Endpoint -
    # - localhost/livros (GET)
    # - localhost/livros (POST)
    # - localhost/livros/id (GET)
    # - localhost/livro/id (PUT)
    # - localhost/livro/id (DELETE)
# 4. Quais recusos - Livros

from flask import Flask, jsonify, request

app = Flask(__name__)

livros = [
    {
        'id': 1,
        'título': 'O senhor dos Anéis - A Sociedade do Anel',
        'autor': 'J.R.R Tolkien'
    },
    {
        'id': 2,
        'título': 'Harry Potter e a Pedra Filosofal',
        'autor': 'J.K Howling'
    },
    {
        'id': 3,
        'título': 'Hábitos Atômicos',
        'autor': 'James Clear'
    },
]

# Consultar(todos)
@app.route('/livros',methods=['GET'])
def obter_livros():
    return jsonify(livros)
# Consultar(id)
@app.route('/livros/<int:id>',methods=['GET'])
def obter_livro_porid(id):
    for livro in livros:
      if livro.get('id') == id:
         return jsonify(livro)
# Editar
@app.route('/livros/<int:id>',methods=['PUT'])
def editar_livro_por_id(id):
   livro_alterado = request.get_json()
   for indice, livro in enumerate(livros):
      if livro.get('id') == id:
         livros[indice].update(livro_alterado)
         return jsonify(livros[indice])
# Criar
@app.route('/livros',methods=['POST'])
def incluir_novo_livro():
   novo_livro = request.get_json()
   livros.append(novo_livro)

   return jsonify(livros)
# Excluir
@app.route('/livros/<int:id>',methods=['DELETE'])
def excluir_livro(id):
   for indice, livro in enumerate(livros):
      if livro.get('id') == id:
         del livros[indice]

         return jsonify(livros)

app.run(port=5000,host='localhost',debug=True)

