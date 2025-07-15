# SorteioPronto
Um bot Completo e Seguro de criação de sorteios para o Telegram


Fiz este projeto com o intuito de brincar com o módulo telebot (PyTelegramBotAPI). Conforme fui codificando comecei a levar mais a sério
e acabei tornando disto um projeto, no qual trata-se de um Bot Completo, versátil, fácil de usar e com uma ótima aparência destinado a gerenciamento
de sorteios.


#  Controles de Usuários

O bot possui uma verificação de usuários, distinguidos em 2 tipos: Usuários Comuns e Administradores

Caso o ID de um usuário esteja contido na lista de administradores, a restrição de conteúdo não se aplicará. Fazendo com que o usuário
adicionado nesta lista possua visibilidade total de todos os sorteios criados por todos os usuários utilizando o bot.

Caso o ID de um usuário NÃO esteja contido na lista de administradores, a restrição aplicada será
de que os sorteios visíveis e gerenciáveis estejam disponíveis somente aqueles que foram criados pelo ID do usuário em questão
(os sorteios criados pelo próprio usuário)

um trecho do código fonte que exemplifica esta restrição é:
  
    if call.data == 'compartilhar':

        buttons = []
        botao_por_linha = 1
        markup = InlineKeyboardMarkup()
        for id_sorteio, dados in sorteios_ativos.items():
            if call.from_user.id != dados['criador_id'] and call.from_user.id not in admins:
                continue
            nome = dados['nome']
            button = InlineKeyboardButton(f'🔗 {nome}', callback_data=f'compartilhar_{id_sorteio}')
            buttons.append(button)

Esta restrição impede que outros usuários possam deletar, excluir, divulgar ou sortear sorteios de outros usuários.


#  Sistema de Sorteio


Ao utilizar o BOT, você poderá escolher para criar um sorteio.
O BOT irá perguntar se você deseja adicionar uma imagem ao sorteio, caso o usuário escolha que sim, o mesmo deve apenas
enviar uma imagem, no dicionario que armazena as informações do sorteio a chave 'img' deixará de ser None. Com isto, ao executar o evento de enviar o sorteio nos canais,
haverá uma condicional de verificação if que tratará a respeito de img ser None ou Não. Caso seja None, haverá bot.send_message, caso não seja none, haverá bot.send_photo
com o .file_id da imagem que nos servidores do Telegram é um hash que identifica a imagem, permitindo colocá-la em mensagens posteriormente.

Ao selecionar divulgar sorteio você receberá uma lista de sorteios existentes e ativos (caso existam), o usuário selecionará e logo após receberá uma lista de canais para que se divulgue o sorteio.
Quando o usuário clica no canal que deseja sortear, o bot envia a mensagem do sorteio para o channel_id selecionado, a mensagem é enviada com um botão "Participar". Quando um membro do canal interessado em
participar do sorteio clica neste botão, uma caixa aparece em um intervalo de aprox 1.5 segundos informando que esta pessoa adentrou no sorteio, e a mensagem é atualizada dinamicamente conforme o número de participantes.

Na função de sortear, ocorre um random.sample() coontendo o número de participantes total e o número de pessoas sorteadas que fora escolhido durante a criação do sorteio.


Este é apenas um resumo muito simpificado do bot que desenvolvi, no total são 535 linhas que garantem estabilidade, confiabilidade e integridade na função do bot. 

<img src="https://i.postimg.cc/8PBXR4bY/botsorteio-demonstracao.png"></img>

# Vídeo de Demonstração:

https://files.catbox.moe/mv11sr.mp4
