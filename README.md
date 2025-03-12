import random
import matplotlib.pyplot as plt

class GameAgent:
    def __init__(self, difficulty='médio'):
        self.difficulty_levels = {'fácil': (1, 50, 10), 'médio': (1, 100, 7), 'difícil': (1, 200, 5)}
        self.range_min, self.range_max, self.max_attempts = self.difficulty_levels.get(difficulty, (1, 100, 7))
        self.secret_number = random.randint(self.range_min, self.range_max)
        self.attempts = 0
        self.state = "Esperando tentativa"
        self.history = []
  
    def make_guess(self, guess):
        self.attempts += 1
        self.history.append(guess)
        
        if guess == self.secret_number:
            self.state = "Acertou!"
            return "Parabéns! Você acertou!"
        elif self.attempts >= self.max_attempts:
            self.state = "Fim do jogo"
            return f"Game Over! O número era {self.secret_number}."
        elif guess < self.secret_number:
            self.state = "Tentativa errada (muito baixo)"
            return "O número é maior. Tente novamente. " + self.get_hint(guess)
        else:
            self.state = "Tentativa errada (muito alto)"
            return "O número é menor. Tente novamente. " + self.get_hint(guess)
    
    def get_hint(self, guess):
        """Fornece uma dica com base no desempenho do jogador."""
        diff = abs(self.secret_number - guess)
        if diff > (self.range_max - self.range_min) // 3:
            return "Você está bem longe!"
        elif diff > (self.range_max - self.range_min) // 6:
            return "Você está chegando mais perto!"
        else:
            return "Você está muito perto!"
    
    def show_history(self):
        """Exibe um gráfico com o histórico das tentativas."""
        plt.plot(range(1, len(self.history) + 1), self.history, marker='o', linestyle='-')
        plt.axhline(y=self.secret_number, color='r', linestyle='--', label='Número Secreto')
        plt.xlabel("Tentativas")
        plt.ylabel("Palpites")
        plt.title("Histórico de Tentativas")
        plt.legend()
        plt.show()

# Escolha a dificuldade
difficulty = input("Escolha a dificuldade (fácil, médio, difícil): ").strip().lower()
agent = GameAgent(difficulty)

while agent.state not in ["Acertou!", "Fim do jogo"]:
    try:
        guess = int(input(f"Digite um número entre {agent.range_min} e {agent.range_max}: "))
        print(agent.make_guess(guess))
    except ValueError:
        print("Entrada inválida. Digite um número inteiro.")

# Mostrar o histórico ao final do jogo
agent.show_history()
