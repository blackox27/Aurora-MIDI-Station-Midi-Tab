[LEIA-ME.txt](https://github.com/user-attachments/files/34126874/LEIA-ME.txt)
MIDI SHARE 2.0 — AURALIS, AURORA E MIDI TAB COM CONTAS PRIVADAS

COMECE AQUI
1. Extraia TODO o ZIP para uma pasta nova.
2. Tenha Python 3.10 ou superior no computador servidor; recomenda-se 3.12+.
3. No Windows, execute Iniciar-Windows.bat. Não é necessário instalar pacotes.
4. O navegador abrirá o login. Clique em Criar uma conta.
5. Escolha um usuário (3–32 caracteres) e senha (10–128 caracteres).
6. Entre na conta e escolha Auralis, Aurora ou MIDI Tab no portal.

O servidor utiliza a biblioteca padrão do Python, inclusive SQLite e hashlib.
Os outros computadores precisam apenas de navegador e acesso ao servidor.
Não existe usuário ou senha padrão. Não há recuperação de senha por e-mail.

AURALIS → AURORA OU MIDI TAB
1. Abra Auralis pelo portal.
2. Importe seu áudio e faça a transcrição; ou abra um projeto Auralis salvo.
3. Revise as notas. Aguarde qualquer análise terminar.
4. Clique no botão Rede MIDI, no canto inferior direito.
5. Clique em Enviar música atual. O MIDI será guardado na SUA conta.
6. Na lista, clique em Abrir no Aurora ou Abrir no MIDI Tab.
   A abertura no Aurora pede confirmação para substituir seu projeto atual.
7. No outro computador, entre na MESMA conta para receber esse arquivo.

O áudio e os projetos Auralis não são enviados por esse fluxo. Apenas o arquivo
MIDI é transferido. O Auralis continua processando o áudio no navegador.
A qualidade da transcrição e os limites dos programas de destino permanecem
os de seus motores originais. Samples e SoundFonts não fazem parte do MIDI.

PRIVACIDADE POR CONTA
- As listagens, os downloads, os metadados e os envios exigem login.
- Um usuário não acessa MIDI de outro, mesmo conhecendo seu ID ou link.
- O servidor determina a conta pelo cookie de sessão, nunca por um campo de
  usuário enviado junto do arquivo.
- Repetições de upload são identificadas dentro da conta, sem conflito com
  os envios de outra pessoa.
- Sair revoga a sessão. As outras abas são avisadas quando há suporte a
  BroadcastChannel; verificações de sessão e cabeçalhos vinculam a página
  à conta original e impedem enviar um projeto por uma aba de outra conta.
- O salvamento local do Aurora utiliza uma chave de armazenamento por conta.
- Salve projetos portáteis antes de sair, recarregar ou trocar de conta.

Os dados no navegador e no disco não são criptografados. A separação de contas
protege o acesso pela aplicação e API; não impede o administrador do computador
servidor de administrar os arquivos. Em computadores compartilhados com pessoas
não confiáveis, use perfis separados do navegador e do sistema operacional.

SENHAS E CONEXÃO
Senhas são guardadas como hash PBKDF2-HMAC-SHA256 com salt aleatório, sem texto
puro. Cookies de sessão usam HttpOnly e SameSite=Strict. Há proteção CSRF,
expiração e limite de tentativas. Uma sessão dura até 12 horas, com limite de
30 minutos sem atividade. Há no máximo cinco sessões por conta.

HTTP não criptografa senhas nem arquivos em trânsito. Para demonstração local,
abra http://localhost:8000. Para proteger credenciais durante uso na rede,
configure HTTPS com certificado e chave próprios, válidos para o endereço usado:

python iniciar.py --tls-cert "certificado.pem" --tls-key "chave.pem" --open

No modo HTTPS, o cookie também recebe Secure. Os certificados não acompanham o
pacote e devem ser confiáveis nos computadores clientes. Não publique este
servidor didático diretamente na internet.

EM DOIS COMPUTADORES
- Conecte ambos à mesma rede local.
- Inicie o servidor apenas em um deles e mantenha o terminal aberto.
- O terminal mostra possíveis endereços IPv4 para acesso pela rede.
- No outro computador, use esse endereço, e não localhost.
- Entre na conta desejada. Outra conta terá outra biblioteca.
- Se necessário, permita Python na rede privada no firewall do servidor.

ATUALIZAÇÃO DA VERSÃO 1.x
Encerre o servidor antigo. Faça uma cópia de segurança de dados/midis.
Extraia a versão 2.0 em outra pasta. Ela não atribui os arquivos antigos à
primeira pessoa que fizer login: não há como conhecer automaticamente seu dono.

Para importar os arquivos antigos para uma conta escolhida:
1. Execute a versão 2.0, crie a conta e encerre o servidor.
2. Copie os pares .mid/.json antigos para dados/midis da instalação nova.
3. No terminal, execute: python iniciar.py --import-legacy-to seu.usuario
4. Inicie novamente. Os originais ficam preservados e a conta recebe cópias.
Repetir o comando confirma as mesmas importações, sem duplicá-las.

Não execute dois servidores usando a mesma pasta de dados. Para fazer backup,
encerre o servidor e copie dados/midis inteiro: contas.sqlite3 e usuarios também
são necessários para preservar as contas e seus arquivos privados.

OUTRAS OPÇÕES
Outra porta: python iniciar.py --port 8001 --open
Somente este computador: python iniciar.py --host 127.0.0.1 --open
Outra pasta de dados: python iniciar.py --data "C:\meus-dados-midi"
Cotas por conta: python iniciar.py --max-files 2000 --max-total-mib 512 --open
Linux/macOS: sh iniciar-linux-mac.sh
Encerrar: Ctrl+C no terminal.

ORGANIZAÇÃO
servidor/auth.py       Contas, hash de senha, sessões e limitação de tentativas
servidor/server.py     HTTP/HTTPS, autorização e endpoints
servidor/storage.py    Catálogos e persistência de MIDIs
servidor/midi.py       Validação MIDI
web/                  Portal, login e três aplicativos
web/shared/           Integração de rede e controle de sessão
web/auralis/          Auralis 1.2.0 integrado, completo, com fontes e licenças
web/aurora/           Aurora corrigido integrado
web/midi-tab/         MIDI Tab integrado
aurora-codigo-original/ Fontes e licenças originais do Aurora corrigido
dados/midis/          Banco de contas, diretórios privados e arquivos legados
docs/                 Documentação acadêmica, técnica e de atualização
tests/                Testes de isolamento, API, integração e preservação

LIMITES
MIDI tipo 0/1, PPQ, até 32 MiB. Até 50 arquivos por fila, 2.000 arquivos e 512 MiB
por conta por padrão. A listagem pode atualizar a cada 5 segundos. Não há edição
colaborativa nem reprodução sincronizada entre computadores. A fila de pendentes
fica na memória da aba e desaparece quando ela é fechada. O Auralis não recebe
MIDIs para transcrever: ele gera MIDI a partir de áudio/projetos. O arquivo gerado
pode ser aberto no Aurora e no MIDI Tab. Alguns recursos de áudio do Aurora
precisam de contexto seguro e navegador compatível.

Consulte docs/ATUALIZACAO-2.0.md, docs/SEGURANCA-2.0.md e docs/TESTES.md.
