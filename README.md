# FilmeJava
import tkinter as tk
from tkinter import ttk
from tkinter import messagebox

class MovieApp(tk.Tk):
    def __init__(self):
        super().__init__()

        self.title("Aplicativo de Filmes")
        self.geometry("600x400")

        self.filmes = []
        self.filme_selecionado = None

        self.id_usuario = None  # Identificador do usuário

        self.estilo = ttk.Style()
        self.estilo.theme_use("clam")

        self.estilo.configure("Orange.TButton", background="orange", foreground="black", font=("Helvetica", 10))
        self.estilo.map("Orange.TButton", background=[("active", "orange1")])

        self.estilo.configure("Purple.TFrame", background="purple")
        self.estilo.configure("TNotebook", background="purple")
        self.estilo.configure("TNotebook.Tab", background="purple", foreground="white", padding=[10, 5])

        self.criar_abas()

    def criar_abas(self):
        aba_controle = ttk.Notebook(self)
        
        aba_filmes = ttk.Frame(aba_controle, style="Purple.TFrame")
        aba_avaliacoes = ttk.Frame(aba_controle, style="Purple.TFrame")
        aba_sinopse = ttk.Frame(aba_controle, style="Purple.TFrame")
        aba_configuracoes = ttk.Frame(aba_controle, style="Purple.TFrame")
        aba_mensagens = ttk.Frame(aba_controle, style="Purple.TFrame")
        aba_sobre = ttk.Frame(aba_controle, style="Purple.TFrame")

        aba_controle.add(aba_filmes, text="Filmes")
        aba_controle.add(aba_avaliacoes, text="Avaliações")
        aba_controle.add(aba_sinopse, text="Sinopse")
        aba_controle.add(aba_configuracoes, text="Configurações")
        aba_controle.add(aba_mensagens, text="Mensagens")
        aba_controle.add(aba_sobre, text="Sobre")

        aba_controle.pack(expand=1, fill='both')

        self.criar_aba_filmes(aba_filmes)
        self.criar_aba_avaliacoes(aba_avaliacoes)
        self.criar_aba_sinopse(aba_sinopse)
        self.criar_aba_configuracoes(aba_configuracoes)
        self.criar_aba_mensagens(aba_mensagens)
        self.criar_aba_sobre(aba_sobre)

    def criar_aba_filmes(self, aba_filmes):
        rotulo = ttk.Label(aba_filmes, text="Adicionar um Filme:", style="Purple.TLabel")
        rotulo.grid(row=0, column=0)

        self.entrada_filme = ttk.Entry(aba_filmes, width=30)
        self.entrada_filme.grid(row=0, column=1)

        botao_adicionar = ttk.Button(aba_filmes, text="Adicionar", command=self.adicionar_filme, style="Orange.TButton")
        botao_adicionar.grid(row=0, column=2)

        self.lista_filmes = tk.Listbox(aba_filmes, width=50)
        self.lista_filmes.grid(row=1, columnspan=3)

    def adicionar_filme(self):
        nome_filme = self.entrada_filme.get()
        if nome_filme:
            self.filmes.append(nome_filme)
            self.lista_filmes.insert(tk.END, nome_filme)

    def criar_aba_avaliacoes(self, aba_avaliacoes):
        rotulo = ttk.Label(aba_avaliacoes, text="Avalie o Filme (1 a 5 estrelas):", style="Purple.TLabel")
        rotulo.pack()

        self.lista_avaliacoes = tk.Listbox(aba_avaliacoes, width=50)
        self.lista_avaliacoes.pack()

        self.escala_avaliacao = ttk.Scale(aba_avaliacoes, from_=1, to=5, orient=tk.HORIZONTAL)
        self.escala_avaliacao.pack()

        botao_avaliar = ttk.Button(aba_avaliacoes, text="Avaliar", command=self.avaliar_filme, style="Orange.TButton")
        botao_avaliar.pack()

    def avaliar_filme(self):
        if not self.lista_avaliacoes.curselection():
            messagebox.showwarning("Aviso", "Por favor, selecione um filme para avaliar.")
            return

        nome_filme = self.lista_avaliacoes.get(self.lista_avaliacoes.curselection())
        avaliacao = self.escala_avaliacao.get()
        print(f"Avaliou '{nome_filme}' com {avaliacao} estrelas")

    def criar_aba_sinopse(self, aba_sinopse):
        rotulo = ttk.Label(aba_sinopse, text="Digite a Sinopse:", style="Purple.TLabel")
        rotulo.grid(row=0, column=0)

        self.entrada_sinopse = tk.Text(aba_sinopse, wrap=tk.WORD, width=50, height=10)
        self.entrada_sinopse.grid(row=1, column=0, columnspan=3)

    def criar_aba_configuracoes(self, aba_configuracoes):
        rotulo_idioma = ttk.Label(aba_configuracoes, text="Idioma:", style="Purple.TLabel")
        rotulo_idioma.grid(row=0, column=0, sticky="w")
        self.variavel_idioma = tk.StringVar(aba_configuracoes)
        self.variavel_idioma.set("Português")
        opcoes_idioma = ["Português", "Inglês", "Espanhol"]
        menu_idioma = ttk.OptionMenu(aba_configuracoes, self.variavel_idioma, *opcoes_idioma)
        menu_idioma.grid(row=0, column=1, sticky="w")

        rotulo_tema = ttk.Label(aba_configuracoes, text="Tema:", style="Purple.TLabel")
        rotulo_tema.grid(row=1, column=0, sticky="w")
        self.variavel_tema = tk.StringVar(aba_configuracoes)
        self.variavel_tema.set("Claro")
        opcoes_tema = ["Claro", "Escuro"]
        menu_tema = ttk.OptionMenu(aba_configuracoes, self.variavel_tema, *opcoes_tema)
        menu_tema.grid(row=1, column=1, sticky="w")

        rotulo_perfil = ttk.Label(aba_configuracoes, text="Perfil:", style="Purple.TLabel")
        rotulo_perfil.grid(row=2, column=0, sticky="w")
        self.entrada_perfil = ttk.Entry(aba_configuracoes)
        self.entrada_perfil.grid(row=2, column=1, sticky="w")

        botao_adicionar_amigo = ttk.Button(aba_configuracoes, text="Adicionar Amigo", command=self.adicionar_amigo, style="Orange.TButton")
        botao_adicionar_amigo.grid(row=3, column=0, columnspan=2)

    def adicionar_amigo(self):
        id_amigo = self.entrada_perfil.get()
        if id_amigo:
            print(f"Amigo adicionado: {id_amigo}")
        else:
            messagebox.showwarning("Aviso", "Por favor, insira um ID de usuário.")

    def criar_aba_mensagens(self, aba_mensagens):
        rotulo = ttk.Label(aba_mensagens, text="Enviar uma Mensagem:", style="Purple.TLabel")
        rotulo.grid(row=0, column=0)

        self.entrada_mensagem = ttk.Entry(aba_mensagens)
        self.entrada_mensagem.grid(row=0, column=1)

        botao_enviar = ttk.Button(aba_mensagens, text="Enviar", command=self.enviar_mensagem, style="Orange.TButton")
        botao_enviar.grid(row=0, column=2)

        self.area_mensagens = tk.Text(aba_mensagens, wrap=tk.WORD, width=50, height=10)
        self.area_mensagens.grid(row=1, column=0, columnspan=3)

    def enviar_mensagem(self):
        mensagem = self.entrada_mensagem.get()
        if mensagem:
            self.area_mensagens.insert(tk.END, f"Você: {mensagem}\n")
            self.entrada_mensagem.delete(0, tk.END)
        else:
            messagebox.showwarning("Aviso", "Por favor, insira uma mensagem.")

    def criar_aba_sobre(self, aba_sobre):
        rotulo_versao = ttk.Label(aba_sobre, text="Versão: 1.0", style="Purple.TLabel")
        rotulo_versao.pack()

        rotulo_desenvolvedor = ttk.Label(aba_sobre, text="Desenvolvedor: Seu Nome", style="Purple.TLabel")
        rotulo_desenvolvedor.pack()

if __name__ == "__main__":
    app = MovieApp()
    app.mainloop()
