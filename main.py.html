import os
import json
import logging
import datetime
from aiogram import Bot, Dispatcher, types
from aiogram.utils import executor

# Налаштування логування
logging.basicConfig(level=logging.INFO)

# Змінні середовища
TELEGRAM_TOKEN = os.getenv('TELEGRAM_TOKEN')
SMM_CHAT_ID = int(os.getenv('SMM_CHAT_ID', '0'))
DATA_DIR = os.getenv('DATA_DIR', '/data')
DATA_FILE = os.path.join(DATA_DIR, 'leaderboard.json')

# Перевірка обов'язкових параметрів
if not TELEGRAM_TOKEN or not SMM_CHAT_ID:
    logging.error("Відсутній TELEGRAM_TOKEN або SMM_CHAT_ID")
    exit(1)

# Створюємо папку для збереження даних
os.makedirs(DATA_DIR, exist_ok=True)

# Завантажуємо або ініціалізуємо дані
if os.path.exists(DATA_FILE):
    with open(DATA_FILE, 'r', encoding='utf-8') as f:
        data = json.load(f)
else:
    data = {}

# Ініціалізація бота
token = TELEGRAM_TOKEN
bot = Bot(token=token)
dp = Dispatcher(bot)
user_state = {}

# Функція збереження даних
def save_data():
    with open(DATA_FILE, 'w', encoding='utf-8') as f:
        json.dump(data, f, ensure_ascii=False, indent=2)

# /start команда
@dp.message_handler(commands=['start'])
async def cmd_start(message: types.Message):
    kb = types.ReplyKeyboardMarkup(resize_keyboard=True)
    kb.add("Весільний контент", "Корпоративний контент")
    await message.answer("Привіт! Що ти сьогодні знімав?", reply_markup=kb)

# Вибір категорії
@dp.message_handler(lambda m: m.text in ["Весільний контент", "Корпоративний контент"])
async def choose_category(message: types.Message):
    user_state[message.from_user.id] = message.text
    await message.answer("Будь ласка, надішліть ЛІНК на публікацію або сторіс (не файл).")

# Обробка випадкових файлів/відео: відхилити
@dp.message_handler(content_types=["video", "photo", "document"])
async def reject_file_upload(message: types.Message):
    await message.answer("Я зберігаю лише текстові лінки на контент. Будь ласка, надішліть ЛІНК, а не файл.")

# Прийом тільки лінків
@dp.message_handler(lambda m: m.text and m.text.startswith("http"))
async def receive_link(message: types.Message):
    uid = str(message.from_user.id)
    name = message.from_user.full_name
    content_type = user_state.get(message.from_user.id, "Невідомо")
    link = message.text.strip()
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    entry = data.get(uid, {"name": name, "count": 0, "subs": []})
    entry['count'] += 1
    entry['subs'].append({"type": content_type, "link": link, "time": timestamp})
    data[uid] = entry
    save_data()

    # Підтвердження автору
    await message.answer(f"Контент зафіксовано! У тебе вже {entry['count']} підвантажень.")

    # Повідомлення SMM-команді
    alert = (
        f"🆕 Новий контент від {name}\n"
        f"Тип: {content_type}\n"
        f"Лінк: {link}\n"
        f"Час: {timestamp}"
    )
    await bot.send_message(chat_id=SMM_CHAT_ID, text=alert)

# /stats команда для топ-10
@dp.message_handler(commands=['stats'])
async def cmd_stats(message: types.Message):
    top = sorted(data.items(), key=lambda x: x[1]['count'], reverse=True)[:10]
    txt = "🏆 Топ 10 учасників:\n"
    for i, (_, info) in enumerate(top, 1):
        txt += f"{i}. {info['name']}: {info['count']}\n"
    await message.answer(txt)

# Старт бота
if __name__ == '__main__':
    executor.start_polling(dp, skip_updates=True)