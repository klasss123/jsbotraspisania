const TelegramApi = require('node-telegram-bot-api')
const XLSX = require('xlsx')
const token = '6781131766:AAEQ76-288n3I0DwIi16V7d0Lqg9SWyxf64'
//var worksheet = XLSX.readFile('./Table1.xlsx');

const bot = new TelegramApi(token, {polling: true})

//console.log(workbook.Sheets)
const filePath = './Table1.xlsx'

const workbook = XLSX.readFile(filePath);
const sheetName = workbook.SheetNames[0];
const worksheet = workbook.Sheets[sheetName];

const dataArray = XLSX.utils.sheet_to_json(worksheet, {
header: 1,
range: "A1:J2000",
raw: false,
defval: "",
blankrows: false
     });


const groupstud = {};
const groupInputFlag = {};
const Options = {
    reply_markup: JSON.stringify({
        inline_keyboard: [
            [{text: 'Понедельник', callback_data: 'пн'}, {text: 'Вторник', callback_data: 'вт'}, {text: 'Среда', callback_data: 'ср'}],
            [{text: 'Четверг', callback_data: 'чт'}, {text: 'Пятница', callback_data: 'пт'}, {text: 'Суббота', callback_data: 'сб'}],
            [{text: 'четное', callback_data: 'чет'}, {text: 'нечетное', callback_data: 'неч'}],
        ]


    })
}

    bot.setMyCommands([
        {command: '/start', description: 'Запуск бота'},
        {command: '/help', description: 'Мануал бота'},
        {command: '/raspisania', description: 'Расписание'},
        {command: '/group', description: 'Номер группы *Обязательно'}
        ])
        
        bot.on('message', async msg => {
            const text = msg.text;
            const chatId = msg.chat.id;
        console.log(msg);
            if (text === '/start' || text === '/start@Raspisania_pool_bot')
            {
               
               await bot.sendMessage(chatId, `Добро пожаловать в расписание, ${msg.from.first_name}`)
              
            }
        
            if (text === '/help' || text === '/help@Raspisania_pool_bot')
            {
                await bot.sendMessage(chatId, 'Здесь инфо как надо сделать')
            }

            if (text === '/raspisania' || text === '/raspisania@Raspisania_pool_bot')
            {
                await bot.sendMessage(chatId, 'Выберите дни недели', Options)
                
            }


        })

        bot.on('callback_query', msg => {
            const data = msg.data;
            const chatId = msg.message.chat.id;

            if (data == 'пн' || data == 'вт' || data == 'ср' || data == 'чт' || data == 'пт' || data == 'сб')
            {
              dayN = data
            }

            if (data == 'чет' || data == 'неч')
            {
                dayT = data
            }

            if ( dayN && dayT)
            {
                bot.sendMessage(chatId, `выбрана ${dayN}, ${dayT}`);
            
                const filteredData = dataArray.filter(row =>
                    Number(row[0]) === savedGroupNumber &&
                    row[1].replace(/\s/g, '').includes(dayN) &&
                    new RegExp(dayT).test(row[3])
                  );
                  
                  
                  if (dayN == 'ср')
                  {
                  bot.sendMessage(chatId, `Группа: 4334 \nДень: ср\nВремя: 11:20\nПредмет: Безопастность жизнедеятельность\nДата: неч/чет \nАудитория: 525 \nВид занятие: пр\nПреподователь ИВОЛГА ИГРОЬ АЛЕКСАНДРОВИЧ `)
                  }
                  if (dayN == 'пн')
                  {
                    bot.sendMessage(chatId, `Группа: 4334 \nДень: пн\nВремя: 11:20\nПредмет: Физическая культура\nДата: неч/чет \nАудитория:КСК Алимп \nВид занятие: пр\nПреподователь: Фиг поймеш `)
                  }

                  const formattedData = filteredData.map(row => {
                    return `Группа: ${row[0]}\nДень: ${row[1].trim()}\nВремя: ${row[2]}\nПредмет: ${row[4]}\nДата: ${row[3]}\nАудитория: ${row[6]}\nВид занятие: ${row[5]}\nПреподаватель: ${row[9].trim()}\n -------------------------------------------------`;
                  });
                  
                  const messageText = `Отфильтрованные данные:\n${formattedData.join("\n")}`;
                  
                  

                
            
                
                
                bot.sendMessage(chatId, messageText);
                console.log(msg); 
            }
        
            
        });
    

        let savedGroupNumber;

        bot.on('text', async (msg) => {
            const text = msg.text;
            const chatId = msg.chat.id;
            if (text === '/group@Raspisania_pool_bot')
            {
                savedGroupNumber = 4334
            }else
            {

                if (text === '/group') 
                {
                    groupInputFlag[chatId] = true;
                    await bot.sendMessage(chatId, 'Напишите номер группы');
                } else if (groupInputFlag[chatId]) {
                    const enteredNumber = parseInt(text, 10);
            
                    if (!isNaN(enteredNumber) && /^\d{4}$/.test(enteredNumber.toString())) {
                        groupstud[chatId] = enteredNumber;
                        groupInputFlag[chatId] = false;
                        await bot.sendMessage(chatId, 'Номер группы сохранен!');
            
                        savedGroupNumber = groupstud[chatId];
                        await bot.sendMessage(chatId, `Сохраненный номер группы: ${savedGroupNumber}`);
                    } else {
                        await bot.sendMessage(chatId, 'Неверный формат номера группы');
                    }
                }

            }
        
           
        });
        
      
        
