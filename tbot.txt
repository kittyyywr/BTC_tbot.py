import telebot
import requests
from telebot import types
from datetime import datetime

bot = telebot.TeleBot('****************************************)


def telegram_bot(token):
    bot = telebot.TeleBot(token)

@bot.message_handler(commands=['start'])
def start_message(message):
    bot.send_message(message.chat.id, "Hello friend type (/help) to bring up the menu!")

@bot.message_handler(commands=['help'])
def btc(message):
    markup = types.ReplyKeyboardMarkup(resize_keyboard=True,row_width=3)
    start = types.KeyboardButton('/start')
    help = types.KeyboardButton('/help')
    BTC = types.KeyboardButton('price')
    about = types.KeyboardButton('/about')
    markup.add(start,help,BTC,about)
    bot.send_message(message.chat.id,"menu launched",reply_markup=markup)

@bot.message_handler(commands=['about'])
def start_message(message):
        bot.send_message(message.chat.id, "We are a community of developers who want to try themselves in all directions.From simple website layout to creating something big! For example: games, mobile applications, information systems",parse_mode = 'html')

def get_data():
    req = requests.get("https://yobit.net/api/3/ticker/btc_usd")
    response = req.json()
    sell_price = response["btc_usd"]["sell"]
    print(f"{datetime.now().strftime('%Y-%m-%d %H:%M')}\n Sell BTC price {sell_price}")

@bot.message_handler(content_types=['text'])
def send_text(message):
    if message.text.lower() == "price":
        try:
            req = requests.get('https://yobit.net/api/3/ticker/btc_usd')
            response = req.json()
            sell_price = response["btc_usd"]["sell"]
            bot.send_message(
                message.chat.id,
                f"{datetime.now().strftime('%Y-%m-%d %H:%M')}\n Sell BTC price {sell_price} USD"
            )
        except Exception as ex:
            print(ex)
            bot.send_message(
                message.chat.id,
                "Something went wrong..."
            )
    else:
        bot.send_message(message.chat.id, "Check the commands you enter...")

bot.polling(none_stop=True)