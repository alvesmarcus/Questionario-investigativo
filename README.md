Questionário investigativo do projeto integrador Full Stack 

Identificação 

Disciplina: Laboratório de Programação Full Stack 

Data: 05/11/2026 Turma: A  Grupo: __________ 

Integrante 

Matrícula 

1. Cauã Pereira Menezes   

202411283 

2. Marcus Vinicius Alves de Oliveira 

202411258 

3. Nathan de Faria Alves Alvarenga 

202312034 

4. Roberto Kleber da Fonseca Ponce Junio 

202411320 

 

Regra de resposta 

Cada resposta deve apresentar: ação executada, evidência coletada, arquivo e função localizados e explicação da relação entre o código e o resultado. Respostas apenas conceituais não atendem à atividade. 

Parte 1 Inicialização e arquitetura 

1. Inicie a aplicação e registre a mensagem do terminal que prova que o servidor está disponível. Localize no comando executado o módulo e o objeto carregados pelo Uvicorn. 

Ação executada: Iniciamos o projeto pelo terminal do VS Code usando o comando do Uvicorn. 

Evidência: Print mostrando que o servidor iniciou corretamente no endereço http://127.0.0.1:8000. 

Arquivo e função: Arquivo backend/app/main.py, aplicação app. 

Explicação: Servidor foi iniciado com sucesso e está funcionando. Também consegui acessar algumas páginas da API sem apresentar erros. 

2. Abra a interface e o Swagger. Registre os dois endereços e explique por que ambos pertencem à mesma aplicação. 

Ação executada: Abrimos a interface do projeto e também o Swagger pelo navegador. 

Evidência: Prints mostrando a interface em http://127.0.0.1:8000 e o Swagger em http://127.0.0.1:8000/docs. 

Arquivo e função: backend/app/main.py, responsável por iniciar a aplicação e disponibilizar as rotas. 

Explicação: Os dois endereços fazem parte do mesmo projeto. A primeira página mostra o sistema de reservas e a segunda permite visualizar e testar as funções da API. 

_ 

3. Localize o `lifespan`. Que instrução ocorre antes do `yield`? Mostre no código onde a tabela é criada e explique por que reiniciar não apaga os registros. 

Ação executada: Procuramos a função lifespan no arquivo main.py. 

Evidência: Print mostrando a função lifespan e a chamada criar_tabela(). 

Arquivo e função: backend/app/main.py, função lifespan. 

Explicação: Antes do yield, o sistema executa criar_tabela(), que cria a tabela no banco de dados. Os registros continuam salvos mesmo quando reinicio o servidor porque ficam armazenados no arquivo SQLite. 

4. Desenhe o fluxo de uma criação desde o clique em Salvar até a gravação no SQLite. Para cada etapa, informe o arquivo correspondente. 

Ação executada: Preencher os dados da reserva na tela do sistema e cliquei no botão Salvar. 

Evidência: Print da interface mostrando o formulário de cadastro de reservas. 

Arquivo e função: frontend/app.js, backend/app/main.py (função criar_reserva), backend/app/services.py (função criar_reserva) e backend/app/database.py. 

Explicação:  Ao clicar em Salvar, os dados são enviados pelo frontend para a API. A API recebe as informações, chama a função responsável por criar a reserva e os dados são salvos no banco SQLite. 

Parte 2 Contrato da API 

5. No Swagger, identifique método, URL, corpo JSON, resposta e código HTTP da criação. Relacione cada item ao conceito de contrato da API. 

Ação executada: Ao brir o Swagger, localizamos o método POST /api/reservas e observei os dados necessários para criar uma reserva. 

Evidência: Print do Swagger mostrando o POST, o corpo JSON e a resposta da criação. 

Arquivo e função: backend/app/main.py, função criar_reserva. 

Explicação: método utilizado é o POST e a URL é /api/reservas. O corpo JSON contém os dados da reserva, como laboratório, data, turno, responsável, quantidade e finalidade. A resposta retorna os dados da reserva criada. O código HTTP é 201, indicando que o cadastro foi realizado com sucesso. Essas informações fazem parte do contrato da API, pois mostram como enviar os dados e o que esperar como resposta. 

 

6. Crie uma reserva válida pela interface. Registre a requisição no painel Rede do navegador ou repita a operação no Swagger. Localize a chamada `fetch`, a rota FastAPI e o modelo Pydantic envolvidos. 

Ação executada:  Criamos uma nova reserva pelo Swagger, preenchendo os dados necessários. 

Evidência:  Print mostrando a requisição POST, os dados enviados e a resposta 201. 

Arquivo e função:  backend/app/main.py, função criar_reserva. 

Explicação: Enviamos os dados pelo Swagger, a API recebeu as informações e salvou a nova reserva no banco de dados. 

7. Explique por que a API devolve `201` e não `200` na criação. Localize a configuração desse código no backend e o teste que o verifica. 

Ação executada: Procuramos no código onde está definido o código HTTP da criação de reservas. 

Evidência: Print mostrando a configuração status_code=status.HTTP_201_CREATED 

Arquivo e função: backend/app/main.py, função criar_reserva. 

Explicação: A API retorna 201 porque esse código significa que um novo registro foi criado com sucesso. O código 200 é usado para indicar que uma solicitação foi realizada com sucesso, mas o 201 é mais adequado para criação de novos registros. 

 

8. Envie `quantidade_pessoas` igual a zero pelo Swagger. Registre o `422`, identifique a informação de erro e localize a restrição correspondente no modelo. 

Ação executada:  Enviamos uma reserva pelo Swagger com a quantidade de pessoas igual a zero. 

Evidência: Print mostrando o erro 422 e a mensagem de validação. 

Arquivo e função: backend/app/schemas.py, modelo ReservaEntrada. 

Explicação: O sistema não aceita quantidade igual a zero porque existe uma regra de validação que exige uma quantidade maior que zero. Por isso, a API retorna o erro 422. 

9. Consulte um identificador inexistente. Registre o `404`, localize a decisão no endpoint e explique por que essa situação pertence à API, embora a busca ocorra no serviço. 

Ação executada:  Abrimos o Swagger e consultei uma reserva usando um ID que não existe no sistema. 

Evidência: Print do Swagger mostrando a requisição GET e o erro 404 (Reserva não encontrada). 

Arquivo e função: backend/app/main.py, função consultar_reserva. 

Explicação:  A API procura a reserva usando o ID informado. Quando não encontra nenhum registro, retorna o erro 404. A busca é feita pelo serviço, mas a API é responsável por informar que a reserva não foi encontrada. 

Parte 3 Persistência e regras 

10. Crie uma reserva, encerre o servidor, reinicie e consulte novamente. Registre a evidência de permanência e localize a instrução SQL responsável pela gravação. 

Ação executada:  Criamos uma reserva, parei o servidor, iniciei novamente e consultei a reserva pelo Swagger. 

Evidência:  Print mostrando a reserva sendo encontrada depois de reiniciar o servidor. 

Arquivo e função: backend/app/database.py, função responsável por salvar a reserva no banco de dados. 

Explicação: Fizemos o teste para verificar se os dados continuavam salvos depois de reiniciar o servidor. A reserva permaneceu no sistema porque as informações ficam armazenadas no banco SQLite. 

 

11. Cadastre duas reservas com o mesmo laboratório, data e turno. Registre a primeira resposta e a segunda. Localize a verificação do conflito, a conversão da exceção em `409` e o teste automatizado correspondente. 

Ação executada: Tentamos cadastrar duas reservas para o mesmo laboratório, na mesma data e no mesmo turno. A primeira foi aceita e a segunda apresentou erro 409. 

Evidência:  Print do Swagger mostrando a segunda tentativa e a mensagem Conflito: laboratório já reservado nessa data e turno. 

Arquivo e função:   

backend/app/services.py: verificação de conflito. 

backend/app/main.py: função criar_reserva, que transforma o erro em resposta HTTP 409. 

tests/test_api.py ou tests/test_services.py: teste automatizado do conflito. 

 

Explicação:  O sistema não permite duas reservas para o mesmo laboratório, data e turno. Quando tento cadastrar uma reserva repetida, a API identifica o conflito e retorna o erro 409, informando que aquele horário já está ocupado. 

 

12. Altere apenas o responsável de uma reserva. Registre o `PUT`, o JSON devolvido e as instruções do frontend, endpoint e serviço que participam do fluxo. 

Ação executada:  Acessamos o Swagger, selecionei o método PUT e informei o ID da reserva que desejava atualizar. 

Evidência: Print do Swagger mostrando o método PUT, o ID da reserva e os dados enviados. 

Arquivo e função:  backend/app/main.py, função atualizar_reserva, e backend/app/services.py. 

Explicação: O método PUT permite atualizar uma reserva existente. Nesse teste, alterei o responsável e mantive as outras informações da reserva. A API recebe os dados e envia para o serviço realizar a atualização no banco de dados. 

 

13. Exclua uma reserva e tente consultá-la. Registre `204` e depois `404`. Explique por que uma resposta `204` não possui corpo e localize o tratamento desse caso no JavaScript. 

Ação executada:  Excluimos uma reserva pelo Swagger usando o método DELETE e depois tentei consultar o mesmo ID pelo método GET. 

Evidência:  Prints mostrando a exclusão com código 204 e a consulta posterior com código 404. 

Arquivo e função:  backend/app/main.py, função excluir_reserva, e frontend/app.js, responsável por executar a exclusão pelo JavaScript. 

Explicação:  O código 204 indica que a reserva foi excluída com sucesso e não possui corpo de resposta porque não precisa retornar nenhum dado. Quando tento consultar novamente a mesma reserva, a API retorna 404 porque o registro não existe mais. 

 

14. Use o filtro de turno. Registre a URL com query string, o resultado exibido e a montagem dinâmica da consulta SQL. 

Ação executada:  Acessamos o Swagger e utilizei o filtro de turno para mostrar somente as reservas da manhã. 

Evidência:  Print mostrando a URL com o filtro ?turno=manha e o resultado das reservas encontradas. 

Arquivo e função:  backend/app/main.py, função listar_reservas, e backend/app/services.py, função listar_reservas. 

Explicação:  O sistema recebe o turno escolhido pela URL e utiliza essa informação para buscar somente as reservas daquele período. No código, a consulta SQL é montada de acordo com o filtro escolhido, utilizando AND turno = ? para selecionar os registros. 

 

15. Abra o resumo antes e depois de criar uma reserva noturna. Registre a mudança e localize a função que calcula `total` e `por_turno`. 

Ação executada:  Consultamos o resumo das reservas antes e depois de cadastrar uma nova reserva nem um determinado horario. 

Evidência:  Prints do Swagger mostrando o resumo antes e depois da criação da reserva. 

Arquivo e função:  backend/app/services.py, função obter_resumo. 

Explicação:  A função obter_resumo() calcula a quantidade total de reservas e também separa os registros por turno. Depois de cadastrar uma reserva noturna, o total e a quantidade do turno da noite aumentaram em 1. 

 

Parte 4 Testes automatizados 

16. Execute `python -m pytest -v`. Registre o total de testes, o tempo e os estados apresentados. Explique como o pytest descobriu os arquivos e as funções. 

Ação executada:  Executamos o comando python -m pytest -v no terminal do VS Code para verificar os testes do projeto. 

Evidência:  Print do terminal mostrando que o pytest encontrou 7 testes e executou todos com sucesso. 

Arquivo e função: pytest.ini, pasta tests e funções de teste dos arquivos test_api.py e test_services.py. 

Explicação:  O pytest procura automaticamente os arquivos de teste dentro da pasta configurada. Ele identifica os arquivos e as funções que começam com test_ e executa os testes encontrados. 

 

 

17. Escolha o teste do fluxo CRUD. Separe no código os trechos Arrange, Act e Assert, mesmo que não estejam comentados com esses nomes. 

Ação executada:  Analisamos o teste responsável por criar, consultar, atualizar e excluir uma reserva. 

Evidência:  Print do arquivo tests/test_api.py, mostrando o teste test_fluxo_criar_consultar_atualizar_excluir. 

Arquivo e função:  tests/test_api.py, função test_fluxo_criar_consultar_atualizar_excluir. 

Explicação:  O teste é dividido em três partes: Arrange, onde são preparados os dados; Act, onde são realizadas as operações; e Assert, onde são verificadas as respostas para saber se tudo funcionou corretamente. 

18. Explique por que `TestClient` não exige Uvicorn. Indique o caminho percorrido por uma chamada `client.post` até a resposta testada. 

Ação executada:  Analisamos como os testes utilizam o TestClient para fazer requisições à API. 

Evidência:  Print do arquivo de testes mostrando a utilização do TestClient. 

Arquivo e função:  tests/conftest.py, configuração do cliente de testes, e tests/test_api.py. 

Explicação:  O TestClient permite testar a API diretamente, sem precisar iniciar o servidor Uvicorn. Ele envia a requisição para a aplicação FastAPI, recebe a resposta e permite verificar se o resultado está correto. 

 

19. Localize `tmp_path`, `monkeypatch` e `with TestClient(app) as cliente`. Explique o papel de cada elemento e apresente uma evidência de que o banco normal não foi alterado pela suíte. 

Ação executada 

Analisamos a configuração dos testes para entender como o banco de dados é separado durante a execução. 

Evidência 

Print do arquivo tests/conftest.py mostrando tmp_path, monkeypatch e with TestClient(app) as cliente. 

Arquivo e função 

tests/conftest.py, configuração das fixtures de teste. 

Explicação 

O tmp_path cria uma pasta temporária para os testes, o monkeypatch permite alterar configurações durante a execução e o TestClient permite testar a aplicação sem iniciar o servidor normalmente. Assim, os testes podem utilizar um banco separado sem alterar o banco principal do projeto. 

 

20. Modifique temporariamente uma asserção correta para provocar uma falha. Registre a mensagem do pytest, identifique valor obtido e esperado e depois restaure o teste. 

Ação executada:  Modificamos temporariamente uma verificação do teste para provocar um erro e executei o pytest novamente. Depois restaurei o código original. 

Evidência:  Print do terminal mostrando a falha e a mensagem do pytest. 

Arquivo e função:  tests/test_api.py, teste escolhido para realizar a alteração. 

Explicação:  Fiz uma alteração proposital em uma verificação para observar como o pytest mostra os erros. Ele informa o resultado esperado e o resultado obtido, facilitando a identificação do problema. Depois restaurei a verificação original. 

 

21. Execute a cobertura. Informe o percentual total, escolha duas linhas não cobertas e proponha cenários de teste capazes de executá-las. 

Ação executada: Executamos o comando python -m pytest --cov=backend.app --cov-report=term-missing para verificar a cobertura dos testes. 

Evidência:  Print do terminal mostrando o relatório de cobertura com o resultado total de 94%. 

Arquivo e função:  Arquivos backend/app/main.py e backend/app/services.py. 

Explicação:  A cobertura mostra quanto do código foi executado durante os testes. No meu projeto, o resultado foi de 94%. Algumas linhas ainda não foram executadas, então seria possível criar novos testes para verificar essas partes do sistema. 

22. Compare `test_rejeitar_conflito` com `test_servico_impede_conflito`. Explique a diferença entre testar pela API e testar diretamente a regra de negócio. 

Ação executada:  Comparamos os testes test_rejeitar_conflito e test_servico_impede_conflito. 

Evidência: Print dos dois testes nos arquivos de teste do projeto. 

Arquivo e função: Arquivos tests/test_api.py e tests/test_services.py. 

Explicação:  O teste pela API verifica se o sistema retorna o erro 409 quando existe uma reserva no mesmo laboratório, data e turno. Já o teste do serviço verifica diretamente se a regra de negócio consegue identificar esse conflito. Um testa a resposta da API e o outro testa a lógica interna do sistema. 

 

Parte 5 Síntese do fluxo 

23. Complete a cadeia com nomes reais do projeto: evento da interface → função JavaScript → método e rota HTTP → função do endpoint → modelo → função do serviço → SQL → código HTTP → atualização da interface. 

Ação executada:  Analisamos o caminho que os dados percorrem desde o cadastro de uma reserva na interface até serem salvos no banco de dados. 

Evidência:  Prints do formulário, do código JavaScript, da rota no main.py e da gravação no banco de dados. 

Arquivo e função:  frontend/app.js, backend/app/main.py (criar_reserva), backend/app/schemas.py (ReservaEntrada), backend/app/services.py (criar_reserva) e backend/app/database.py. 

Explicação:  Quando clicamos em Salvar, o JavaScript envia os dados para a API. A API recebe e verifica as informações, depois o serviço salva a reserva no banco de dados. Por fim, o sistema retorna uma resposta de sucesso e atualiza a tela mostrando a reserva cadastrada. 

 

24. Escolha um defeito possível. Informe em qual camada ele surgiria, qual evidência o revelaria e qual teste deveria impedir sua volta. 

Ação executada:  Analisei um possível erro no cadastro de reservas, relacionado à criação de duas reservas para o mesmo laboratório, data e turno. 

Evidência:  Print do Swagger mostrando o erro 409 quando tentei cadastrar uma reserva em um horário que já estava ocupado. 

Arquivo e função:  backend/app/services.py, função existe_conflito, e backend/app/main.py, função criar_reserva. 

Explicação:  Um possível defeito seria o sistema permitir duas reservas para o mesmo laboratório, na mesma data e turno. Esse erro aconteceria na camada de serviços, caso a verificação de conflito não funcionasse. O teste test_servico_impede_conflito deveria identificar esse problema e impedir que ele acontecesse novamente. 

Entrega 

Questionário preenchido. 

Capturas numeradas E01 a E12 conforme o roteiro de evidências. 

Arquivo de texto com comandos executados. 

Indicação de arquivo, função e trecho de código em todas as questões que solicitam localização. 

 

Roteiro de evidências 

As evidências devem mostrar somente a área necessária para comprovar o resultado. Cada imagem deve conter uma legenda curta informando ação, resultado e questão relacionada. 

Código 

Evidência mínima 

Questões 

E01 

Terminal com Uvicorn iniciado 

1 

E02 

Interface e endereço no navegador 

2 

E03 

Swagger com as rotas disponíveis 

2 e 5 

E04 

Criação válida com resposta 201 e JSON 

6 e 7 

E05 

Validação rejeitada com 422 

8 

E06 

Consulta inexistente com 404 

9 

E07 

Registro existente após reiniciar 

10 

E08 

Duas tentativas e resposta 409 

11 

E09 

Atualização por PUT 

12 

E10 

Exclusão 204 e consulta 404 

13 

E11 

Filtro e resumo alterado 

14 e 15 

E12 

pytest e relatório de cobertura 

16 a 22 

 

Registro de confronto entre execução e código 

Para cada evidência, informe: 

1. O que foi executado. 

2. O que foi observado. 

3. Qual arquivo e função explicam o resultado. 

4. Qual trecho confirma a explicação. 

5. Qual teste protege esse comportamento ou qual teste deveria ser criado. 
