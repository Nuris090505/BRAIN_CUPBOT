import asyncio
from aiogram import Bot, Dispatcher, F
from aiogram.types import (
    Message,
    FSInputFile,
    ReplyKeyboardMarkup,
    KeyboardButton,
    InlineKeyboardMarkup,
    InlineKeyboardButton,
)
from aiogram.filters import CommandStart
from aiogram.fsm.state import State, StatesGroup
from aiogram.fsm.context import FSMContext
from aiogram.fsm.storage.memory import MemoryStorage

TOKEN = "8646216978:AAFizvVOnlbUO1h53NZbNqpy1YbQmQrJTDo"

# 2 АДМИН ID
ADMINS = [7874914629, 7966226119]

bot = Bot(token=TOKEN)
dp = Dispatcher(storage=MemoryStorage())


# ---------------- STATES ----------------

class RegisterState(StatesGroup):
    faction = State()
    speaker1 = State()
    speaker2 = State()

class QuestionState(StatesGroup):
    question = State()

class ComplaintState(StatesGroup):
    complaint = State()

class ComplimentState(StatesGroup):
    compliment = State()


# ---------------- КНОПКАЛАР ----------------

main_menu = ReplyKeyboardMarkup(
    keyboard=[
        [KeyboardButton(text="1-БАС ТӨРЕШІ")],
        [KeyboardButton(text="2-БАС ҰЙЫМДАСТЫРУШЫ")],
        [KeyboardButton(text="3-БАС ГИД")],
        [KeyboardButton(text="4-БАС КООРДИНАТОР")],
        [KeyboardButton(text="5-ТУРНИР ТУРАЛЫ")],
    ],
    resize_keyboard=True
)

tournament_menu = ReplyKeyboardMarkup(
    keyboard=[
        [KeyboardButton(text="1-СҰРАҚ ҚОЮ")],
        [KeyboardButton(text="2-ПІКІР ШАҒЫМ")],
        [KeyboardButton(text="3-КОМПЛИМЕНТ")],
    ],
    resize_keyboard=True
)


# ---------------- START ----------------

@dp.message(CommandStart())
async def start(message: Message, state: FSMContext):

    photo = FSInputFile("C:/Users/Wakh/Desktop/braincup.jpg")

    await message.answer_photo(
        photo=photo,
        caption="ФРАКЦИЯ АТАУЫ"
    )

    await state.set_state(RegisterState.faction)


# ---------------- РЕГИСТРАЦИЯ ----------------

@dp.message(RegisterState.faction)
async def faction_input(message: Message, state: FSMContext):

    await state.update_data(faction=message.text)

    await message.answer("Спикерлер есімі?\n\n1 спикер -")

    await state.set_state(RegisterState.speaker1)


@dp.message(RegisterState.speaker1)
async def speaker1_input(message: Message, state: FSMContext):

    await state.update_data(speaker1=message.text)

    await message.answer("2 спикер -")

    await state.set_state(RegisterState.speaker2)


@dp.message(RegisterState.speaker2)
async def speaker2_input(message: Message, state: FSMContext):

    await state.update_data(speaker2=message.text)

    photo = FSInputFile("C:/Users/Wakh/Desktop/BR1.jpg")

    await message.answer_photo(
        photo=photo,
        caption="BRAIN CUP I турниріне қош келдіңіз!!!",
        reply_markup=main_menu
    )

    await state.clear()


# ---------------- 1 ----------------

@dp.message(F.text == "1-БАС ТӨРЕШІ")
async def chief_judge(message: Message):

    photo = FSInputFile("C:/Users/Wakh/Desktop/photo_2026-05-08_13-56-05.jpg")

    text = """
БАС ТӨРЕШІ

Аты: NURALI KORKEM
Номер: +7 747 667 0181

"""

    await message.answer_photo(photo=photo, caption=text)


# ---------------- 2 ----------------

@dp.message(F.text == "2-БАС ҰЙЫМДАСТЫРУШЫ")
async def organizer(message: Message):

    photo = FSInputFile("C:/Users/Wakh/Desktop/MADI.jpg")

    text = """
БАС ҰЙЫМДАСТЫРУШЫ

Аты: KEMELBEK MADIYAR
Номер: +7 776 012 0515

"""

    await message.answer_photo(photo=photo, caption=text)


# ---------------- 3 ----------------

@dp.message(F.text == "3-БАС ГИД")
async def chief_shid(message: Message):

    photo = FSInputFile("C:/Users/Wakh/Desktop/гид.jpg")

    text = """
БАС ГИД

Аты: ORYNKHAN BEKSULTAN
Номер: +7 702 971 7987

"""

    await message.answer_photo(photo=photo, caption=text)


# ---------------- 4 ----------------

@dp.message(F.text == "4-БАС КООРДИНАТОР")
async def coordinator(message: Message):

    photo = FSInputFile("C:/Users/Wakh/Desktop/координатор.jpg")

    text = """
БАС КООРДИНАТОР

Аты: AYAZBEK AIZERE
Номер: +7 775 330 6116

"""

    await message.answer_photo(photo=photo, caption=text)


# ---------------- 5 ----------------

@dp.message(F.text == "5-ТУРНИР ТУРАЛЫ")
async def tournament(message: Message):

    await message.answer(
        "Қажетті бөлімді таңдаңыз:",
        reply_markup=tournament_menu
    )


# ---------------- СҰРАҚ ----------------

# ТЕК АДМИН REPLY

# ---------------- СҰРАҚ ----------------

@dp.message(F.text == "1-СҰРАҚ ҚОЮ")
async def ask_question(message: Message, state: FSMContext):

    await message.answer("ӨЗ СҰРАҒЫҢЫЗДЫ ЕНГІЗІҢІЗ")

    await state.set_state(QuestionState.question)


@dp.message(QuestionState.question)
async def get_question(message: Message, state: FSMContext):

    user = message.from_user

    admin_text = (
        f"📩 ЖАҢА СҰРАҚ\n\n"
        f"👤 @{user.username}\n"
        f"🆔 {user.id}\n\n"
        f"❓ {message.text}"
    )

    for admin in ADMINS:

        sent_message = await bot.send_message(
            admin,
            admin_text
        )

    await message.answer("✅ СҰРАҒЫҢЫЗ ЖІБЕРІЛДІ")

    await state.clear()


# ---------------- АДМИН REPLY ----------------

@dp.message(F.reply_to_message)
async def admin_reply(message: Message):

    # ТЕК АДМИН
    if message.from_user.id not in ADMINS:
        return

    replied_text = message.reply_to_message.text

    if not replied_text:
        return

    # USER ID АЛУ
    if "🆔" not in replied_text:
        return

    try:
        user_id = replied_text.split("🆔")[1].split("\n")[0].strip()

        await bot.send_message(
            int(user_id),
            f"📨 АДМИН ЖАУАБЫ:\n\n{message.text}"
        )

        await message.answer("✅ ЖАУАП ЖІБЕРІЛДІ")

    except Exception as e:
        print(e)

# ---------------- ШАҒЫМ ----------------

# ---------------- ШАҒЫМ ----------------

@dp.message(F.text == "2-ПІКІР ШАҒЫМ")
async def complaint_start(message: Message, state: FSMContext):

    await message.answer("ПІКІРІҢІЗДІ ЕНГІЗІҢІЗ")

    await state.set_state(ComplaintState.complaint)


@dp.message(ComplaintState.complaint)
async def get_complaint(message: Message, state: FSMContext):

    user = message.from_user

    admin_text = (
        f"⚠️ ЖАҢА ШАҒЫМ\n\n"
        f"👤 @{user.username}\n"
        f"🆔 {user.id}\n\n"
        f"📝 {message.text}"
    )

    for admin in ADMINS:

        await bot.send_message(
            admin,
            admin_text
        )

    await message.answer(
        "ПІКІРІҢІЗГЕ КӨП РАҚМЕТ, СІЗБЕН АДМИНДЕР БАЙЛАНЫСАДЫ"
    )

    await state.clear()
    
# ---------------- ADMIN REPLY ----------------

# ---------------- АДМИН REPLY ----------------

@dp.message(F.reply_to_message)
async def admin_reply(message: Message):

    if message.from_user.id not in ADMINS:
        return

    replied_text = message.reply_to_message.text

    if not replied_text:
        return

    if "🆔" not in replied_text:
        return

    try:
        user_id = replied_text.split("🆔")[1].split("\n")[0].strip()

        await bot.send_message(
            int(user_id),
            f"📨 АДМИН ЖАУАБЫ:\n\n{message.text}"
        )

        await message.answer("✅ ЖАУАП ЖІБЕРІЛДІ")

    except Exception as e:
        print(e)

# ---------------- КОМПЛИМЕНТ ----------------

@dp.message(F.text == "3-КОМПЛИМЕНТ")
async def compliment(message: Message, state: FSMContext):

    await message.answer("КОМПЛИМЕНТ ЖАЗЫҢЫЗ")

    await state.set_state(ComplimentState.compliment)


@dp.message(ComplimentState.compliment)
async def compliment_send(message: Message, state: FSMContext):

    text = f"""
❤️ ЖАҢА КОМПЛИМЕНТ

👤 @{message.from_user.username}

💬 {message.text}
"""

    for admin in ADMINS:
        await bot.send_message(admin, text)

    await message.answer("КӨП КӨП РАҚМЕТ!!!")

    await state.clear()


# ---------------- ADMIN REPLY ----------------

@dp.message(F.reply_to_message)
async def admin_reply(message: Message):

    if message.from_user.id not in ADMINS:
        return

    replied_text = message.reply_to_message.text

    if "🆔" not in replied_text:
        return

    try:
        user_id = int(replied_text.split("🆔")[1].split("\n")[0].strip())

        await bot.send_message(
            user_id,
            f"📨 АДМИН ЖАУАБЫ:\n\n{message.text}"
        )

        await message.answer("ЖАУАП ЖІБЕРІЛДІ!")

    except:
        pass


# ---------------- RUN ----------------

async def main():
    await dp.start_polling(bot)

if __name__ == "__main__":
    asyncio.run(main())
