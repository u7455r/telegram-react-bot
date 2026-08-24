import os
import random
import time
import telebot
from telebot import types
from flask import Flask
from threading import Thread

# ==========================================
# ১. ওয়েব সার্ভার (Render ২৪/৭ সচল রাখার জন্য)
# ==========================================
app = Flask('')

@app.route('/')
def home():
    return "Auto Bot is Live and Running 24/7!"

def run():
    port = int(os.environ.get('PORT', 8080))
    app.run(host='0.0.0.0', port=port)

def keep_alive():
    t = Thread(target=run)
    t.start()

# ==========================================
# ২. টেলিগ্রাম বট কনফিগারেশন
# ==========================================
BOT_TOKEN = "8767626217:AAHXHhGQEVFgPqJElNauMwpyBiV-WEQ7KJk"
bot = telebot.TeleBot(BOT_TOKEN, threaded=False)

# রিয়্যাকশন তালিকা
REACTIONS = ["🔥", "❤️", "👍", "👏", "🎉", "🥰", "⚡", "😍", "🥳", "💯"]

# বাংলা কমেন্ট তালিকা
MESSAGES = [
    "অসাধারণ একটি পোস্ট! অনেক ধন্যবাদ। ❤️",
    "দারুণ কালেকশন! পোস্টটি বেশ ভালো লাগলো। 🔥",
    "সবাই পোস্টটিতে লাইক ও রিঅ্যাকশন দিয়ে সাথেই থাকুন! 👍",
    "প্রতিদিনের সেরা সব আপডেটের জন্য আমাদের সাথেই থাকুন। ✨",
    "পোস্টটি ভালো লাগলে বন্ধুদের সাথে বেশি বেশি শেয়ার করুন! 📢",
    "অসাধারণ কনটেন্ট! পরবর্তী পোস্টের অপেক্ষায় রইলাম। ⚡",
    "সবাই কেমন আছেন? পোস্টটি কেমন লাগলো কমেন্টে জানাতে পারেন! 🥰",
    "চ্যানেলে সক্রিয় থাকার জন্য সবাইকে অসংখ্য ধন্যবাদ! 🎉",
    "সেরা পোস্ট! সবসময় এরকম দারুণ কনটেন্ট চাই। 💯",
    "আপনাদের ভালোবাসা ও সাপোর্টই আমাদের অনুপ্রেরণা! 💖"
]

@bot.message_handler(commands=['start'])
def send_welcome(message):
    welcome_text = (
        f"👋 **হ্যালো {message.from_user.first_name}!**\n\n"
        "🤖 আমি আপনার **Auto Reaction & Comment Bot**।"
    )
    bot.reply_to(message, welcome_text, parse_mode="Markdown")

# ==========================================
# ৩. চ্যানেলে অটো রিঅ্যাকশন
# ==========================================
@bot.channel_post_handler(content_types=['text', 'photo', 'video', 'document', 'audio', 'voice'])
def handle_channel_post(message):
    chosen_emoji = random.choice(REACTIONS)
    try:
        reaction_type = types.ReactionTypeEmoji(chosen_emoji)
        bot.set_message_reaction(
            chat_id=message.chat.id,
            message_id=message.message_id,
            reaction=[reaction_type],
            is_big=False
        )
    except Exception as e:
        print(f"Reaction error: {e}")

# ==========================================
# ৪. ডিসকাশন গ্রুপে অটো কমেন্ট
# ==========================================
@bot.message_handler(func=lambda msg: msg.is_automatic_forward == True)
def handle_discussion_comment(message):
    chosen_msg = random.choice(MESSAGES)
    try:
        bot.reply_to(message, chosen_msg)
    except Exception as e:
        print(f"Auto Comment Error: {e}")

# ==========================================
# ৫. বট স্টার্ট
# ==========================================
if __name__ == '__main__':
    keep_alive()
    
    try:
        bot.remove_webhook()
    except Exception:
        pass
        
    time.sleep(2)
    print("Bot is successfully running...")
    bot.infinity_polling(timeout=20, long_polling_timeout=10, skip_pending=True)
