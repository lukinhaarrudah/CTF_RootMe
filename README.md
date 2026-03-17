# 🚩 RootMe - TryHackMe Write-up

![Difficulty](https://img.shields.io/badge/Dificuldade-Fácil-green?style=for-the-badge)
![Category](https://img.shields.io/badge/Categoria-Web--Exploitation-blue?style=for-the-badge)

Documentação técnica da exploração da máquina **RootMe**. Este laboratório foca em técnicas de **Reconhecimento Web**, **Bypass de Upload** e **Escalação de Privilégios em Linux**.

---

## 🔍 1. Reconhecimento & Enumeração

A fase inicial consistiu em identificar portas abertas e serviços ativos no alvo.

* **Nmap Scan:** Identifiquei as portas 22 (SSH) e 80 (HTTP) utilizando varredura com Nmap , utilizei o --min-rate pois queria um scan mais rapido e como era um ctf mais simples imaginei que não teriam mecanismos de bloqueio ativos.

  ![Scan Nmap](img/nmap.png)

* **Enumeração Web:** Utilizei o `GoBuster` para buscar diretórios ocultos, localizando os caminhos cruciais `/panel/` e `/uploads/`, comecei análise do código-fonte de cada página em busca de comentários, scripts ou dicas estruturais que pudessem revelar vetores de ataque.

  ![GoBuster Results](img/gobuster.png)

---

## ⚡ 2. Exploração: Vulnerabilidade de Upload e RCE

O objetivo nesta etapa foi obter a execução remota de código (RCE) através do formulário de upload.

1. **Acesso ao Painel:** Localizei a página de upload onde os arquivos seriam enviados, então pensei em tentar enviar arquivos .php para conseguir uma shell.
 
   ![Pagina Panel](img/pagina%20panel.png)

2. **Tentativa e Proteção:** Ao tentar subir uma shell padrão, o servidor apresentou um bloqueio para a extensão `.php`.
 
   ![Protecao PHP](img/protecao_php.png)

3. **Técnica de Bypass:** Alteri a extensão da reverse shell para  `.PHp5`, o que permitiu contornar o filtro e realizar o upload.

    ![Shell Enviada](img/shell_enviada.png)
   ![Shell no Diretorio](img/mostrando_shell_no_diretorio.png)

5. **Ganho de Acesso:** Preparei o listener com Netcat e executei o arquivo pelo navegador para receber a conexão.
   ![Listener NC](img/ligando%20nc%20443.png)
   ![Obtendo Shell](img/obtendo_shell_nc.png)

6. **Flags Iniciais:** Já dentro do sistema, primeiramente consegui uma shell melhor... utilizando o comando: python -c 'pty;pty.spawn("/bin/bash")' e encontrando a primeira flag no diretorio www.
 
   ![User Flag](img/obtendo_user_txt.png)

---

## 🚀 3. Escalação de Privilégio (Root)

Com o acesso inicial, busquei elevar meus privilégios para o usuário root.

* **Busca por SUID:** Procurei por binários com permissões especiais e encontrei o Python2.7.

  ![Busca SUID](img/encontrando_python_pra_priv.png)

* **Elevação Final:** Utilizei o Python para invocar um shell de root e ler a flag final do desafio.

   ![Root Flag](img/conseguindo_flag_root.png)

---
📝 **Documentação criada por Lucas Arruda Leme**
