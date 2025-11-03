# desafio2

import random

def criar_tabuleiro(linhas, colunas, qtd_minas):
    tabuleiro = [[' ' for _ in range(colunas)] for _ in range(linhas)]


    minas_colocadas = 0
    while minas_colocadas < qtd_minas:
        linha = random.randint(0, linhas - 1)
        coluna = random.randint(0, colunas - 1)
        if tabuleiro[linha][coluna] != '*':
            tabuleiro[linha][coluna] = '*'
            minas_colocadas += 1
    return tabuleiro

def contar_minas_vizinhas(tabuleiro, linha, coluna):
    linhas = len(tabuleiro)
    colunas = len(tabuleiro[0])
    contagem = 0

    for i in range(linha - 1, linha + 2):
        for j in range(coluna - 1, coluna + 2):
            if 0 <= i < linhas and 0 <= j < colunas and not (i == linha and j == coluna):
                if tabuleiro[i][j] == '*':
                    contagem += 1
    return contagem

def mostrar_tabuleiro(tabuleiro, revelado):
    print("\n   " + " ".join([str(i) for i in range(len(tabuleiro[0]))]))
    print("  " + "--" * len(tabuleiro[0]))
    for i in range(len(tabuleiro)):
        linha_exibida = []
        for j in range(len(tabuleiro[0])):
            if revelado[i][j]:
                if tabuleiro[i][j] == '*':
                    linha_exibida.append('*')
                else:
                    linha_exibida.append(str(contar_minas_vizinhas(tabuleiro, i, j)))
            else:
                linha_exibida.append('#')
        print(f"{i}| " + " ".join(linha_exibida))
    print()

def venceu(tabuleiro, revelado):
    for i in range(len(tabuleiro)):
        for j in range(len(tabuleiro[0])):
            if tabuleiro[i][j] != '*' and not revelado[i][j]:
                return False
    return True

def jogar():
    linhas = 5
    colunas = 5
    qtd_minas = 5

    tabuleiro = criar_tabuleiro(linhas, colunas, qtd_minas)
    revelado = [[False for _ in range(colunas)] for _ in range(linhas)]

    while True:
        mostrar_tabuleiro(tabuleiro, revelado)

        try:
            linha = int(input("Digite a linha: "))
            coluna = int(input("Digite a coluna: "))

            if not (0 <= linha < linhas and 0 <= coluna < colunas):
                print("Coordenadas fora do tabuleiro! Tente novamente.\n")
                continue

        except ValueError:
            print("Entrada inválida! Use apenas números.\n")
            continue

        if tabuleiro[linha][coluna] == '*':
            print("\n💥 BOOM! Você pisou em uma mina. Fim de jogo.\n")

            for i in range(linhas):
                for j in range(colunas):
                    revelado[i][j] = True
            mostrar_tabuleiro(tabuleiro, revelado)
            break


        revelado[linha][coluna] = True


        if venceu(tabuleiro, revelado):
            mostrar_tabuleiro(tabuleiro, revelado)
            print("🎉 Parabéns! Você venceu o jogo!\n")
            break

if __name__ == "__main__":
    jogar()
