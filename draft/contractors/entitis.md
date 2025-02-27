давай теперь сделаем сущьность заказа которая будет выхлядить так 
```JSON


{
        orderId: 0,
        // Создание проекта
        itda: 'itda',  // ITDA
        title: 'Название',  // Название
        author: 'Автор',  // Автор
        genre: 'Жанр',  // Жанр
        dateOfCompletion: '22.02.2025', // Плановая дата готовности
        urgentProject: false, // Срочный проект

        // Содержание проекта 
        speaker: false, //Диктор 
        recordingStudio: false, // Запись, студия
        soundEditing: false, // Обработка звука, монтаж
        directing: false, // Режиссура
        mosh: false, // МОШ
        composer: false, // Композитор

        // Тексты и комментарии 
        textSize: 0, // Объем текста 
        attachmentsFiles: [] as any, // Вложения 
        comments: 'Комментарии', // Комментарии

        // Требования к озвучиванию
        authorPerformance: false, // Авторское исполнение 
        polyphonicVoice: false, // Многоголосая озвучка 
        polyphonicVoiceQty: 0, // Количество
        audioadaptation: false, // Необходимость адуио адаптации
        glossary: false, // Необходимость глоссария
        copyrightingAgree: false, // Необходимость согласовать с правообладателем
        attachmentPDF: false, // Наличие pdf приложений
        voiceoverComment: 'Комментария', // Комментария 

        // Требования к диктору 
        recordSeries: false, // Готовность записать серию
        gender: 'Мужской', // Пол
        voicePortrait: 'Портрет голоса', // Портрет голоса  
        ageCharacteristics: 'Возрастная характеристика', //Возрастная характеристика  
        knowledgeLanguages: 'Английский', // Знание языков  
        theaterAtTheMicriphone: false, // Театр у микрофона 
        abilityToImitate: false, // Умение звукоподражать 
        topics: [] as any, // Готовность работать с тематикой 
        additionally: 'Дополнительные требования', // Дополнительные требования

        // Требования к записи и монтажу 
        showReadersName: false, // Читать имя чтеца
        dontVoiceSwearWords: false, // Мат не озвучивать 
        replacementType: 'Тип замены', // Тип замены
        imprintScreensavers: false, // Заставка импринта 
        imprint: 'Выбрать заставку импринта', // Выбрать заставку импринта 
        files: [] as any, // Загрузить файл 
        paddingBetweenChapters: false, // Отбивка между главами
        typePadding: 'Типовая', // Типовая
        intro: false, // Интро
        typeIntro: '', // Типовая
        autro: false, // Аутро
        typeAutro: '', // Типовая

        // Требования к музикально - шумовому оформлению
        musicRequirementList: [] as any, // Требования к музикально - шумовому оформлению
        // Требования к режиссуре 
        directingRequirementsList: [] as any, // Требования к режиссуре 
        // Задание для композитора
        assignmentComposerList: [] as any, // Задание для композитора

        
        // Дополнительные поля
        stageID: 18, // id этап 
        stageName: 'ОК заказчика', // название этапа
        stageArticle: 'okCustomer', // Артикул этапа 
        rate: 3500, // ставка
        changeDate: '22.02.2025',// Дата изменения
        cencelStatus: {
            title: 'Остановлен менеджером',
            statusId: 0,
        }, // статус заказа
        isActiveOrder:  false, // активная зявка 
        term: 20,
        stageStatusName: 'Ждем на проверку', // куда отправлена заявка т ожидание подтверждения, пример если отправлена на проверку  то до подтверждения статус 'ждем на проверку' после подтверждения статус изменится 'на проверку'
        stageStatusDate: '18.02.2025', // дата изменения, когда заказ отправлен на следующий этап, и когда подтвержден на следующем этапе 
        customer: 'ИП Ушаков', 
        agent: 'Сергей Каплунов',
        manager: 'Светлана Александрова',
    }
```

необходимо создать апи который будет создавать и обслуживать сущность project

