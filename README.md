### Arthur Cardoso Rinaldi da Silva

# Ola sou Arthur
Estudante de Banco de Dados, escritor online e estas são minhas experiências na Fatec de Sâo José dos Campos.

# Meus Projetos.

## Em 2019-2
### Descrição:
No primeiro semestre como projeto do API, foi proposto o desenvolvimento de um web bot.
O webotbot "black mamba" tinha como seu objetivo fazer a busca de variação de uma ação no mercado financeiro, afim de entregar ao usuario um momento oportuno para ser feita a compra.
Para isso o bot irá capturar os dados em tempo real do valor de uma ação e comparar com os históricos de variações dessa mesma ação em outros períodos em conjunto com uma mapeamento das notícias que podem influenciar o valor da ação.
O sistema também possuia uma interface vinculada ao power bi, para tornar de forma mais visivel para o usuario como anda a variação da ação e seu historico, alem disso também existia a opção de fazer login pelo telegram.

### Tecnologias: 

* Python 3.7 - Foi escolhido como Linguagem principal pois tinha uma rapida evolução de aprendizado, e como sabiamos que seria necessario uma integração com graficos, a biblioteca "pandas" sairia como uma grande ajuda.
* Zen of Python - Foi passado pelos alunos do 6 semestre para aprendermos mais sobre boas práticas para o Projeto;
* Visual Studio Code - IDE;
* Power Bi - Foi escolhido por ser ser facil de utilizar e ter uma boa evolução de aprendizado;
* MySQL - Foi escolhido por ser o banco de maior conhecimento do grupo;
* Conceitos do SCRUM - Norteador do Projeto.
* Principais Bibliotecas Python:  
    **PyMySQL** - interação com nosso Banco de Dados;  
    **Selenium** - navegação pela Web através do WebDriver do Google Chrome;  
    **Beautiful Soup** - interação com o html dos sites para permitir as raspagens de dados;  
    **Pandas / Matplotlib** - ferramentas para gerar e plotar graficos;  
    **Email / Telegram-bot** - envio de alertas e notificações;
    
 ###  - Contribuições individuais: 

* Tornando o arquivo "executavel.py" executavel.
```python
import sys
from cx_Freeze import setup, Executable
import Classificacao
base = None
if sys.platform == "win64":
    base = "Win64GUI"
Executables = [
    Executable("Classificacao.py", base=base)
]
buildOptions = dict(
    packages = [],
    includes = ["Classificacao"],
    include_files = [],
    excludes = []
)
setup(
    name = "Classificacao",
    version="1.0",
    description = "Espero que funcione",
    options = dict(build_exe = buildOptions),
    Executables = Executables
)
```
Para tornar o arquivo executavel foi utilizado a biblioteca "cx_Freeze".


* Classificacao.py
```python
from selenium import webdriver
from selenium.webdriver.chrome.options import Options
from bs4 import BeautifulSoup
import pymysql
import time
from datetime import datetime
from alert import hahaha
cont = int(1)
trigger_email = []
wait =int(10) # Trocar aqui os tempos de espera - mudar para rodar na FATEC devido a INTERNET lenta
options = Options()
options.add_argument('--headless')
options.add_argument('--disable-gpu')
ff = webdriver.Chrome(options=options)
ff.get('https://economia.uol.com.br/')
while (True):
    while (datetime.now().hour <= 22 and datetime.now().hour >= 10):
        print ('Iniciando',cont,'Âº coleta')
        time.sleep(wait)
        html=ff.page_source
        soup=BeautifulSoup(html,"html.parser")
        time.sleep(2)
        ##-----------------------------------##
        filtro1 = soup.find_all(class_='tv-symbol-price-quote__value js-symbol-last')
        filtro1 = (filtro1[0].text.replace('â','-'))
        trigger_email.append(filtro1)
        ##-----------------------------------
        filtro2 = soup.find_all(class_='js-symbol-change tv-symbol-price-quote__change-value')
        filtro2 = (filtro2[0].text.replace('â','-'))
        ##-----------------------------------
        filtro3 = soup.find_all(class_='js-symbol-change-pt tv-symbol-price-quote__change-value')
        filtro3 = (filtro3[0].text[1:-1].replace('â','-'))
        print (filtro1," / ",filtro2," / ",filtro3,"\n")
       # print("----------------------------------")
        ###Armazenando link com noticias na variavel noticia.
        noticia=soup.find_all("a",{"class": "tv-widget-news tv-widget-news--link js-widget-news-link js-ga-track-news-escape"})
        cont = cont +1
        ff.refresh()
        # 
        connection = pymysql.connect(host='bvzfdagnfqepipz70gyw-mysql.services.clever-cloud.com',
                                     port=3306,
                                     user='ufgpsjx1cswrmye3',
                                     password='ZoKM7HXwAaZAgd9ugpTr')
 
        cursor = connection.cursor()
        cursor.execute("INSERT INTO bvzfdagnfqepipz70gyw.Valor_acoes(Value, IncDec, Variation) VALUES('%s', '%s', '%s')" %(filtro1,filtro2,filtro3))
        cursor.execute("SELECT email FROM bvzfdagnfqepipz70gyw.Usuario")
        q_result = list(cursor.fetchall())
        connection.commit()
        connection.close()
        to_addrs = '' # q_result[0:1].
        for x in list(q_result):
            to_addrs += ''.join(x)
            to_addrs += ''.join('; ')
        if trigger_email[-1:] == trigger_email[-2:-1] and trigger_email[-2:-1] == trigger_email[-3:-2]:
            hahaha("Alerta: AÃ§Ã£o do Inter MUITO valorizada","Cara, compra logo",to_addrs)
            print("Alerta Enviado: Compra Logo")
        else:
            if trigger_email[-1:] == trigger_email[-2:-1]:
                hahaha("Alerta: AÃ§Ã£o do Inter valorizada", "Hora de Comprar",to_addrs)
                print("Alerta Enviado: Compra 1")
```
O classificação.py tinha a função de fazer a coleta de dados do site, armazenar esses dados coletados.
Também envia para o email do usuario informações sobre o quão interessantes as informações estão para o usuário.

<img src="./imgs/powerbi.png" />
Aqui segue uma amostra da interface usada no projeto, nela é possivel filtrar os graficos por mês e por dia.
No gráfico existem 3 gráficos, variação de ações, média de volume por mês e média de valor mensal por ano.
todos esses dados sendo retirados do banco de dados.


### - Hard Skills:
* Python
* Power bi
* MySql
* Scrum
 
###  - Soft Skills: 
*  Proatividade
*  Autonomia
*  Organização

 
## Em 2020-1 

### Descrição
A proposta do projeto é criar uma interface que permita o usuário cadastrar todos os aspectos de seu ambiente de desenvolvimento (pessoas, projetos, tarefas) fazendo com que por meio de uma interface pratica e interativa o usuário consiga elaborar e analisar cenários referente ao planejamento de seu dia-a-dia, orquestrando pessoas, projetos e horas disponíveis de desenvolvimento para potencializar suas entregas.   
Definimos nosso MVP (Minimum Viable Product) como sendo:  

> Permitir que o usuário cadastre todos os aspectos de seu ambiente de desenvolvimento (pessoas, projetos, tarefas) e permitir por meio de uma interface pratica e interativa (drag and drop) que o usuário consiga elaborar e analisar cenários referente ao planejamento de seus projetos e horas disponiveis de desenvolvimento.

### Tecnologias: 

*  Python 3.7 - Linguagem principal - Foi escolhida pelo fator de conhecimento do grupo sobre a linguagem.
*  frappe.io - biblioteca que foi utilizada para criação dos graficos na interface.
*  FrameWork Django 3 - interface WEB - Foi escolhido por ser o framework web de python mais  conhecido.
*  MySQL - Banco de Dados 
*  Conceitos do SCRUM - Norteador do Projeto 
*  Zen of Python - boas práticas para o Projeto  

### Contribuições individuais: 

* extração do banco para salvar no excel
``` python
@login_required
def import_excel(request):
    
        if request.method == 'POST':
            projeto_resource = ProjetoResource()
            dataset = Dataset()
            new_project = request.FILES['myfile']
            imported_data = dataset.load(new_project.read(),format='xlsx')
            #print(imported_data)
            #nome = dataset[0][0]
            #print(nome)
            connection = pymysql.connect(host='localhost',
                             user='bridges_dev',
                             password='123456',
                             db='bridges',
                             charset='utf8mb4',
                             cursorclass=pymysql.cursors.DictCursor)
            for data in imported_data:
                Nprojeto = data[0]
                DescTarefa = data[1]
                HoraTarefa = data[2]
                MinTarefa = data[3]
                try:
                    with connection.cursor() as cursor:
                        sql = "INSERT INTO `bridges_app_projetos` (nom_pro) VALUES (%s)"
                        cursor.execute(sql, (Nprojeto))
                        connection.commit()
                        sql = "select id_pro from bridges_app_projetos where nom_pro = %s order by id_pro limit 1"
                        cursor.execute(sql, (Nprojeto))
                        result = cursor.fetchone()
                        print(result["id_pro"])
                        
                        sql = "INSERT INTO `bridges_app_tarefas` (nom_tar, fk_pro_id, dur_tar_min, dur_tar_hours) VALUES (%s, %s, %s, %s)"
                        print(sql)
                        idpro = result["id_pro"]
                        cursor.execute(sql, (DescTarefa, idpro , MinTarefa, HoraTarefa))
                        connection.commit()
                        messages.success(request, 'Excel importado com Sucesso!')
                except:
                    print(":(")
            
            connection.close()
        return render(request, 'bridges_app/import_excel.html')
```
Primeiro era feito o import do excel na variavel "imported_data", após isso abria a conexão com o banco atribuindo suas credenciais, feito isso existe um for com a varial "imported_data", dentro desse for existem 4 variaveis Nprojeto, DescTarefa, HoraTarefa, MinTarefa. essas variaveis recebiam o valor dos campos do excel atraves do data[numero da coluna].
Após isso é feito a inserção do que foi retirado do excel ao banco.

* Tela de Cadastro de Funcionarios.
<img src="./imgs/tela funcionario.png" />

### Hard Skill: 
* Metodologia Scrum
* Front-end
* Python

### Soft Skill: 
*  Proatividade
*  Autonomia
*  Organização


## Em 2020-2

### Descrição:
Score wizard foi uma aplicação criada com o intuito de ajudar de forma mais pratica e visual de checar sua pontuação de crédito dentro do spc.
Para fazer o calculo do score de cada pessoa, é feita uma conta a partir de todos os dados de transação, empréstimo, financiamento e cartão de crédito da pessoa cadastrados no banco.
Além de exibir a pontuação do score o projeto tambem possuia uma estrategia de gamificação das compras, onde o usuario recebia numero x de tarefas e após concluir elas ele recebe uma quantidade de "xp", atraves disso a ideia é fazer os compradores sempre manterem uma bom score e também de acessar a aplicação.
Nela criamos uma interface onde o usuario pode fazer seu cadastro sendo ele um pessoa fisica com seu "cpf" ou pessoa juridica com seu "cnpj".


### Tecnologias:  
 * MySql Community - Banco utilizado por proximidade a plataforma. 
 * Java 1.8 - Java foi escolhido por ser requisito da intituição de ensino.
 * Spring 2.3.0 - Utilizada para gerenciamento dos dados e tambem para enviar as informaçoes processadas para a interface.
 * Bootstrap 4 - Utilizado para estilizar a interface.
 * Html5 - Utilizado para montar a interface.
 * JavaScript - Utilizado para montar a interface.
 * CSS - Utilizado para estilizar a interface.
    
###  - Contribuições individuais: 
 
### Modelo do Banco

<img src="./imgs/modelagem.png" />
Aqui segue a modelagem utilizada para o projeto.

* Calculo de score para pessoa fisica
```java
  @GetMapping(path="/getPessoaFisicaNewScore")
    public @ResponseBody int getPessoaFisicaNewScore(@RequestParam String documento, @RequestParam ArrayList<Integer> movimentosId) {
        PessoaFisica pessoa = pessoafisicarepository.findByDocumento(documento);
        Double operacao, parcela, atraso, inadimplencia, score;
        operacao = 100.00 / pessoa.getOperacoesCount();
        parcela = 100.00 / pessoa.getMovimentosCount();
        atraso = Double.valueOf(((movimentorepository.findByPessoaFisicaDocumento(pessoa.getDocumento()).size() - movimentosId.size()) * 100) / 100);
        inadimplencia = (parcela * (atraso * 10))/10;
        score = 1000-(inadimplencia*100);
        
        return score.intValue();
    }
```
Nele é possivel notar o calculo por trás do score.
 
### Hard Skill: 
* Java
* Mysql
* Modelagem de banco de dados

### Soft Skill:  
Como aprendizado efetivo é possivel pontuar a criação do modelo do banco, nele nós ja tinhamos um dataset enviado pelo cliente e aproveitamos para adaptalo para o objetivo do projeto, além disso também foi um projeto onde foi possivel aprender muito sobre calculo dentro de um projeto e formas de manipular os dados que estão vindo  do banco.


## Em 2021-1

### Descrição: 
O setor de RH precisa de uma solução parametrizável que combine diversos critérios, para
busca de candidatos em diferentes vagas com diferentes recrutadores numa proposta de
processo eficiente para contratação e evasão de funcionários, reduzindo custos e
aumentando a satisfação com alocações mais adequadas.
Nossa proposta é desenvolver um sistema para a otimização e que facilite o processo de contratação de novos colaboradores , respeitando os requisitos, visando a rapidez e agilidade no processo. Para que esses objetivos sejam atingidos, utilizaremos uma um SGBD orientado a documentos (MongoDB), visando que as estruturas de currículos e vagas são maleáveis e se aproveitam bem da estrutura de documento usada, além de questões de desempenho e funcionalidades que podem ser aproveitadas. 

### Tecnologias: 
* Python 
* django
* mongo db
* postman
* Visual studio code


### Contribuições individuais:
* Modelo de documento do banco
```mongodb
db.Inscrito.insert(
{
    "InscritoIdExterno":"1",
    "rgInscrito":"123.123.123-12",
    "dataNascimentoInscrito":"25/09/2000",
    "sexoInscrito":"masculino",
    "telefoneCelularInscrito":"(12)9816719999",
    "jornadaDesejadaInscrito":"padrão",
    "tipoContratoDesejadoInscrito":"clt",
    "EmailInscrito":"email@email.com",
    "perfilProfissionalTituloInscrito":"full stack developer",
    "perfilProfissionalDescricaoInscrito":"pleno com conhecimento em desenvolvimento full stack com node, dotnet, angular e react",
    "nomeCompletoInscrito":"arthur c",
    "enderecoCEPInscrito":"12235-608",
    "enderecoLocalizacaoInscrito":"rua, bairro",
    "complementoInscrito":"",
    "enderecoLocalizacaoLatitudeInscrito":"",
    "enderecoLocalizacaoLongitudeInscrito":"",
    "experienciaProfissional": [
        {
         "descricao": "Desenvolvimento com front e back end",
         "duracaoTempoExperiencia":"2 anos",
         }
      ],
      "competencia": [
        {
         "descricao": "poliglota",
        }
      ],
    "formacao": [
        {
            "curso":"banco de dados",
            "Dataformacao":"22/06/2020",
            "intituicao":"fatec"
        }
      ],
});
db.vagas.insert(
{
    "VagaIdExterno":"1",
    "tituloVaga":"desc da vaga",
    "tipoContratacaoPerfilVaga":"clt",
    "tipoJornadaPerfilVaga":"liberal",
    "localEnderecoCEPPerfilVaga":"12235-600",
    "localEnderecoPerfilVaga":"rua, bairro",
    "localEnderecoNumeroPerfilVaga":"1234",
    "faixaEtariaInicioPerfilVaga":"21",
    "faixaEtariaFimPerfilVaga":"35",
    "tempoExperienciaPerfilVaga":"2 anos",
    "faixaSalarioInicioPerfilVaga":"2500.00",
    "dataInicioDivulgacaoPerfilVaga":"01/1/2021",
    "datafinaldivulgacaoPerfilVaga":"31/01/2021",
    "competencia": [
        {
         "descricao": "poliglota",
        }
    ],
     "PalavraChave" :[
         {
            "DescricaoPalavraChave":"Chave",
         }
    ],
});
```

* Função utilizada para pegar os curriculos
```python
import json
import requests
import urllib.request
from Mongo_Connection import *

class Finder:

    db_instance = None

    def init(self):
        self.db_instance = Mongo_Connection()

    def search(self, requisitos):
        results = self.db_instance.find_curriculos(requisitos)
        if results.count() == 0:
            print("Nenhum curriculo encontrado...")
        else:
            print("Database response...")
        return results
```

### Hard Skill: 
 * Python
 * mongo db
 * API Rest

### Soft Skill: 
*  Proatividade
*  Autonomia
*  Organização

## Em 2021-2
Neste semestre o problema proposto foi desenvolver uma solução de dados voltada ao ensino a distância para a gestão e oferta do conhecimento, sendo apto a prover suporte às mais variadas arquiteturas de aprendizagem, alinhado com os objetivos estratégicos a serem alcançados pelas organizações que atendermos
como clientes.
Foi necessario ajustar o banco de dados, pensando em um grande processamento de dados com ganho de escalabilidade e
integração contínua entre os ambientes. Adicionar na solução atual um banco de dados não relacional para armazenar os chats e os logs.
Deve ser desenvolvido um pipeline de dados e analytics, a fim de manter um DW e um modelo OLAP para visualização e análise de dados.


### Tecnologias Utilizadas
* Sql
* Java
* Power Bi
* Docker
* Pentaho

### Contribuições Pessoais:

* Interface entregada ao cliente
<img src="./imgs/bi Pythaoff.png" />
Para a entrega dos dados ao cliente, foi escolhido Power bi, visto que era uma ferramenta que o grupo ja tinha uma familiaridade, com essa ferramenta foi possivel puxar os dados direto do banco de dados, assim ficava mais facil para pegar os dados e fazer o calculo dos graficos direto na plataforma.

### Hard Skills
* ETL
* Power Bi

### Soft Skills
*  Proatividade
*  Autonomia
*  Organização


## Em 2022-1
A matéria de Topicos avançados de banco de dados, com a proposta de solucionar um problema relacionado a LGPD.
Para solucionar o problema foi proposto foi particionar uma tabela de venda onde era separado os dados da venda e do cliente, sendo assim possivel apagar os dados do cliente sem precisar apagar a venda.
Neste projeto também foram desenvolvidos 

### Tecnologias Utilizadas
* Pyhton 
* Mongo DB

### Contribuições Pessoais
* Função para particionar a tabela de vendas:
```python
    def Split_Sale():
        cluster = Model.createConnectionDB()
        db = cluster['TopicosAvançados']
        collectionVenda = db['Vendas']
        VendasSeparadas = []
        VendasSeparadas = collectionVenda.find({})
        collectionCli = db['Cliente']
        collectionVendaSimples = db['VendaSimples']
        for dado in VendasSeparadas:
            id = dado['_id']
            produto = dado['produto_venda']
            valor = dado['valor_venda']
            qtd = dado['qtd_venda']
            id_cli = uuid.uuid4().hex
            name = dado['nome_cli']
            telefone = dado['telefone_cli']
            email = dado['email_cli']
            cpf = dado['cpf_cli']
            idChave = dado['id_chave']

            requestCli = { "id": id_cli,
                    "nome_cli":name,
                    "telefone_cli": telefone, 
                    "email_cli": email, 
                    "cpf_cli": cpf,
                    "id_chave": idChave}
            collectionCli.insert_one(requestCli)

            requestVendaSimples = {"_id":id,
                    "produto_venda": produto, 
                    "valor_venda": valor, 
                    "qtd_venda": qtd,
                   "idCli": id_cli}
            collectionVendaSimples.insert_one(requestVendaSimples)
        return HTTPResponse("Tabela Particionada") 
```
O código acima demonstra como foi feita o particionamento do projeto, inicialmente ele pega os dados na tabela de venda e dividi os dados dela em outras 2 tabelas, sendo uma tabela com os dados da venda e o outro com os dados do cliente. Assim sera possivel apagar os dados do cliente quando necessario, sem precisar apagar os dados da venda.

* Tabelas particionadas.
<img src="./imgs/Tabelas topicos avancados.png" />

### Hard Skills
* Particionamento de Banco de dados
* Mongo DB
* Python

### Soft Skills
*  Proatividade
*  Autonomia
*  Organização
