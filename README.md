import random

def criar_tabuleiro(tamanho, num_bombas):
    """
    Cria o tabuleiro do campo minado com as bombas distribuídas aleatoriamente.
    """
    # Tabuleiro vazio
    tabuleiro = [[0 for _ in range(tamanho)] for _ in range(tamanho)]
    
    # Coloca as bombas aleatoriamente
    bombas_colocadas = 0
    while bombas_colocadas < num_bombas:
        linha = random.randint(0, tamanho - 1)
        coluna = random.randint(0, tamanho - 1)
        if tabuleiro[linha][coluna] != "B":  # Evita colocar bombas no mesmo lugar
            tabuleiro[linha][coluna] = "B"
            bombas_colocadas += 1
            
            # Atualiza números ao redor da bomba
            for i in range(-1, 2):
                for j in range(-1, 2):
                    nova_linha = linha + i
                    nova_coluna = coluna + j
                    if 0 <= nova_linha < tamanho and 0 <= nova_coluna < tamanho:
                        if tabuleiro[nova_linha][nova_coluna] != "B":
                            tabuleiro[nova_linha][nova_coluna] += 1
    return tabuleiro

def mostrar_tabuleiro(tabuleiro, revelado):
    """
    Exibe o tabuleiro no terminal, revelando as posições abertas pelo jogador.
    """
    print("\nTabuleiro:")
    tamanho = len(tabuleiro)
    
    # Exibe os números das colunas
    print("  ", end="")
    for i in range(1, tamanho + 1):
        print(i, end=" ")
    print()
    
    for linha in range(tamanho):
        print(linha + 1, end=" ")  # Exibe os números das linhas
        for coluna in range(tamanho):
            if revelado[linha][coluna]:
                print(tabuleiro[linha][coluna], end=" ")
            else:
                print("■", end=" ")  # Quadrado oculto
        print()

def jogar(tamanho=5, num_bombas=5):
    """
    Função principal para jogar o Campo Minado.
    """
    tabuleiro = criar_tabuleiro(tamanho, num_bombas)
    revelado = [[False for _ in range(tamanho)] for _ in range(tamanho)]
    
    while True:
        mostrar_tabuleiro(tabuleiro, revelado)
        try:
            # Solicita ao jogador para selecionar uma posição
            linha = int(input(f"Escolha uma linha (1-{tamanho}): "))
            coluna = int(input(f"Escolha uma coluna (1-{tamanho}): "))
            
            # Ajusta as coordenadas para índices começando em 0
            linha -= 1
            coluna -= 1
            
            # Verifica se as coordenadas estão dentro dos limites
            if not (0 <= linha < tamanho and 0 <= coluna < tamanho):
                print("Coordenadas inválidas! Tente novamente.")
                continue
            
            if tabuleiro[linha][coluna] == "B":
                print("\n💣 BOOM! Você acertou uma bomba! Fim de jogo.")
                mostrar_tabuleiro(tabuleiro, [[True]*tamanho for _ in range(tamanho)])
                break
            
            # Revela a célula
            revelado[linha][coluna] = True
            
            # Verifica se o jogador ganhou (todas as células não-bomba estão reveladas)
            if all(
                revelado[i][j] or tabuleiro[i][j] == "B"
                for i in range(tamanho) for j in range(tamanho)
            ):
                print("\n🎉 Parabéns! Você venceu o jogo!")
                mostrar_tabuleiro(tabuleiro, [[True]*tamanho for _ in range(tamanho)])
                break
        except ValueError:
            print("Entrada inválida! Use apenas números inteiros.")

# Executa o jogo
if __name__ == "__main__":
    print("Bem-vindo ao Campo Minado!")
    tamanho = int(input("Digite o tamanho do tabuleiro (ex: 5 para 5x5): "))
    num_bombas = int(input(f"Digite o número de bombas (máximo {tamanho**2 - 1}): "))
    jogar(tamanho, num_bombas)
