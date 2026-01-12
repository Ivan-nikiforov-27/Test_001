Для запуска проекта локально необходимо:


Убедиться, что установлен Node.js и NPM (или Yarn).

Создать файл .env в корне проекта и добавить туда переменные окружения:

ID_INSTANCE=YOUR_ID_INSTANCE
API_TOKEN_INSTANCE=YOUR_API_TOKEN_INSTANCE
PHONE_NUMBER=YOUR_PHONE_NUMBER

Замените YOUR_ID_INSTANCE, YOUR_API_TOKEN_INSTANCE и YOUR_PHONE_NUMBER на ваши реальные значения из Green-API.

В консоли перейдите в директорию с проектом и выполните команды:


bash
Копировать
npm install # или yarn install
npm test    # или yarn test
Первая команда установит все необходимые зависимости, указанные в package.json. Вторая комманда запустит тесты, которые будут использовать ваши переменные окружения.


Вот пример кода автотестов WhatsApp API на Jest и TypeScript:


typescript
Копировать
// src/whatsapp.test.ts
import { sendMessage, getChatHistory, getStateInstance } from './greenApi';
import { ID_INSTANCE, PHONE_NUMBER } from "./config";

const chatId = `${PHONE_NUMBER}@c.us`; // Формат chatId для WhatsApp

describe('WhatsApp API Tests', () => {

    it('Проверка авторизации инстанса', async () => {
        const response = await getStateInstance();
        expect(response.status).toBe(200);
        expect(response.data.state).toBe('authorized');
    });

    it('Успешная отправка сообщения (200) и проверка idMessage', async () => {
        const response = await sendMessage(chatId, 'Test message');
        expect(response.status).toBe(200);
        expect(response.data).toHaveProperty('idMessage');
    });

    it('Успешное получение истории чата (200)', async () => {
        const response = await getChatHistory(chatId, 10);
        expect(response.status).toBe(200);
    });

    it('Отправка сообщения с некорректными параметрами (400)', async () => {
        try {
            await sendMessage('', '');
        } catch (error: any) {
            expect(error.response.status).toBe(400);
        }
    });

});

В этом примере, переменные окружения импортируются из файла src/config.ts и используются для конфигурации тестов. Каждый тест проверяет определенные аспекты работы API, такие как статус ответа, наличие обязательных полей и корректную обработку ошибок.
