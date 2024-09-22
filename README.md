from datetime import datetime

def mostrar_menu():
    menu = """
    [d] Depositar
    [s] Sacar
    [e] Extrato
    [q] Sair
    => """
    return input(menu)

def depositar(saldo, extrato):
    try:
        valor = float(input("Informe o valor do depósito: "))
        if valor > 0:
            saldo += valor
            extrato.append(f"{datetime.now()} - Depósito: R$ {valor:.2f}")
            print(f"Depósito de R$ {valor:.2f} realizado com sucesso!")
        else:
            print("Operação falhou! O valor informado é inválido.")
    except ValueError:
        print("Operação falhou! Valor inválido.")
    return saldo, extrato

def sacar(saldo, extrato, numero_saques, LIMITE_SAQUES, limite, limite_diario, total_sacado_hoje):
    try:
        valor = float(input("Informe o valor do saque: "))
        excedeu_saldo = valor > saldo
        excedeu_limite = valor > limite
        excedeu_saques = numero_saques >= LIMITE_SAQUES
        excedeu_limite_diario = total_sacado_hoje + valor > limite_diario

        if excedeu_saldo:
            print("Operação falhou! Você não tem saldo suficiente.")
        elif excedeu_limite:
            print("Operação falhou! O valor do saque excede o limite.")
        elif excedeu_saques:
            print("Operação falhou! Número máximo de saques chegou ao limite.")
        elif excedeu_limite_diario:
            print("Operação falhou! Limite diário de saques excedido.")
        elif valor > 0:
            saldo -= valor
            extrato.append(f"{datetime.now()} - Saque: R$ {valor:.2f}")
            numero_saques += 1
            total_sacado_hoje += valor
            print(f"Saque de R$ {valor:.2f} realizado com sucesso!")
        else:
            print("Operação falhou! O valor informado é inválido.")
    except ValueError:
        print("Operação falhou! Valor inválido.")
    return saldo, extrato, numero_saques, total_sacado_hoje

def mostrar_extrato(saldo, extrato):
    print("\n================ EXTRATO ================")
    if not extrato:
        print("Não foram realizadas transações.")
    else:
        for transacao in extrato:
            print(transacao)
    print(f"\nSaldo: R$ {saldo:.2f}")
    print("==========================================")

def main():
    saldo = 0
    limite = 100
    extrato = []
    numero_saques = 100
    LIMITE_SAQUES = 100
    limite_diario = 5000
    total_sacado_hoje = 0

    while True:
        opcao = mostrar_menu()

        if opcao == "d":
            saldo, extrato = depositar(saldo, extrato)
        elif opcao == "s":
            saldo, extrato, numero_saques, total_sacado_hoje = sacar(saldo, extrato, numero_saques, LIMITE_SAQUES, limite, limite_diario, total_sacado_hoje)
        elif opcao == "e":
            mostrar_extrato(saldo, extrato)
        elif opcao == "q":
            break
        else:
            print("Operação inválida, por favor selecione a opção novamente.")

if __name__ == "__main__":
    main()
