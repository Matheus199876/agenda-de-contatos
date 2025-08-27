import json
import os

ARQUIVO = "agenda.json"

# Carregar contatos do arquivo
def carregar_contatos():
    if os.path.exists(ARQUIVO):
        with open(ARQUIVO, "r") as f:
            return json.load(f)
    return []

# Salvar contatos no arquivo
def salvar_contatos(contatos):
    with open(ARQUIVO, "w") as f:
        json.dump(contatos, f, indent=4)

# Adicionar contato
def adicionar_contato(contatos):
    nome = input("Nome: ")
    telefone = input("Telefone: ")
    email = input("Email: ")
    contatos.append({"nome": nome, "telefone": telefone, "email": email})
    print(f"✅ Contato {nome} adicionado!")

# Listar contatos
def listar_contatos(contatos):
    if not contatos:
        print("Nenhum contato na agenda.")
        return
    print("\n--- Contatos ---")
    for i, c in enumerate(contatos, 1):
        print(f"{i}. {c['nome']} | {c['telefone']} | {c['email']}")
    print("----------------\n")

# Buscar contato
def buscar_contato(contatos):
    nome = input("Digite o nome para buscar: ")
    encontrados = [c for c in contatos if nome.lower() in c["nome"].lower()]
    if encontrados:
        for c in encontrados:
            print(f"{c['nome']} | {c['telefone']} | {c['email']}")
    else:
        print("Nenhum contato encontrado.")

# Remover contato
def remover_contato(contatos):
    listar_contatos(contatos)
    try:
        indice = int(input("Digite o número do contato para remover: "))
        contato = contatos.pop(indice - 1)
        print(f"❌ Contato {contato['nome']} removido!")
    except (IndexError, ValueError):
        print("Número inválido.")

# Menu principal
def menu():
    contatos = carregar_contatos()
    while True:
        print("\n--- AGENDA DE CONTATOS ---")
        print("1. Adicionar contato")
        print("2. Listar contatos")
        print("3. Buscar contato")
        print("4. Remover contato")
        print("5. Sair")
        opcao = input("Escolha uma opção: ")

        if opcao == "1":
            adicionar_contato(contatos)
        elif opcao == "2":
            listar_contatos(contatos)
        elif opcao == "3":
            buscar_contato(contatos)
        elif opcao == "4":
            remover_contato(contatos)
        elif opcao == "5":
            salvar_contatos(contatos)
            print("Agenda salva. Até logo!")
            break
        else:
            print("Opção inválida!")

if __name__ == "__main__":
    menu()
