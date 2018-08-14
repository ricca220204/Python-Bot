# Python-Bot
import telepot, sys, time, os

bot_token = "inserisci qui il token del tuo bot"
adminId = 50967453
print("\033[92mRunning\033[0m")
bot = telepot.Bot(bot_token)


def rispondi(msg):
    contentType, chatType, chatId = telepot.glance(msg)
    msgId = msg["message_id"]
    if contentType == "text" and chatType == "private":
        text = msg["text"]
        if chatId != adminId:
            bot.forwardMessage(adminId, chatId, msgId)
        elif "reply_to_message" in msg.keys():
            if "forward_from" in msg["reply_to_message"].keys():
                answerId = msg["reply_to_message"]["forward_from"]["id"]
                bot.sendMessage(answerId, text)

try:
    bot.message_loop({'chat': rispondi})
    while 1:
        time.sleep(5)
except KeyboardInterrupt:
    print("\033[93m\nMi chiudo...\n\033[0m")
    try:
        sys.exit(0)
    except SystemExit:
        os._exit(0)
