# OpenAI
Пример использования асинхронного программирования на python для создания Telegram-Bot-OpenAI


Нужно использовать библиотеку Python aiogram, которая позволяет создавать асинхронные боты для Telegram. Здесь простой пример асинхронных функций для бота, который может использовать OpenAI для генерации текста.

В первую очередь, вам нужно установить aiogram и openai-python, используя pip:

```pip install aiogram openai```

Пример бота:

```ruby
import openai
from aiogram import Bot, Dispatcher, types

openai.api_key = 'YOUR_OPENAI_KEY'
bot = Bot(token='YOUR_BOT_TOKEN')
dp = Dispatcher(bot)
async def ask_gpt3(question):
    response = openai.Completion.create(
      engine="text-davinci-002",
      prompt=question,
      max_tokens=150
    )
    return response.choices[0].text.strip()
    
@dp.message_handler()
async def echo(message: types.Message):
    question = message.text
    answer = await ask_gpt3(question)
    await message.answer(answer)
    
if __name__ == '__main__':
    from aiogram import executor
    executor.start_polling(dp, skip_updates=True)
```
Нужно заменить 'YOUR_OPENAI_KEY' на свой секретный ключ от OpenAI и 'YOUR_BOT_TOKEN' на токен вашего Telegram бота.

Обратите внимание, что эта функция возвращает прямой ответ от GPT-3. В зависимости от того, какой функциональности вы хотите от вашего бота, вам может потребоваться выполнить дополнительную обработку ответа. Кроме того, помните, что использование GPT-3 является платным и имеет определенные ограничения использования, так что убедитесь, что вы понимаете условия использования OpenAI.

Чтобы не было очередей в процессе принятия запроса, необходимо использовать асинхронные сессии используйте библиотеку aiohttp:

```ruby
import aiohttp
import json

async def ask_gpt3(question):
    url = 'https://api.openai.com/v1/engines/davinci-codex/completions'
    headers = {
        'Content-Type': 'application/json',
        'Authorization': 'Bearer YOUR_OPENAI_KEY',
    }
    data = {
        'prompt': question,
        'max_tokens': 150,
    }

    async with aiohttp.ClientSession() as session:
        async with session.post(url, headers=headers, data=json.dumps(data)) as response:
            result = await response.json()
            return result['choices'][0]['text'].strip()
```
            
В этом примере асинхронный клиент aiohttp используется для отправки запроса к API OpenAI и получения ответа. 

