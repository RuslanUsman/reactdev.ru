---
description: В этом уроке вы создадите небольшую игру "Крестики-нолики". Этот учебник не предполагает наличия каких-либо знаний о React
---

# Учебник: крестики-нолики

<big>В этом уроке вы создадите небольшую игру "Крестики-нолики". Этот учебник не предполагает наличия каких-либо знаний о React. Техники, которые вы изучите в этом уроке, являются фундаментальными для создания любого приложения React, и полное понимание этих техник даст вам глубокое понимание React.</big>

!!!note ""

    Этот учебник предназначен для тех, кто предпочитает **учиться на практике** и хочет быстро попробовать сделать что-то осязаемое. Если вы предпочитаете изучать каждую концепцию шаг за шагом, начните с [Описания пользовательского интерфейса](describing-the-ui.md).

Учебник разделен на несколько разделов:

-   Настройка для учебника даст вам **начальную точку** для следования учебнику.
-   Обзор научит вас **основам** React: компонентам, пропсам и состоянию.
-   Завершение игры научит вас **самым распространенным техникам** в разработке React.
-   Добавление путешествия во времени даст вам **более глубокое понимание** уникальных преимуществ React.

## Что вы строите? {#what-are-you-building}

В этом уроке вы создадите интерактивную игру "Крестики-нолики" с помощью React.

Вы можете посмотреть, как она будет выглядеть, когда вы закончите, здесь:

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    function Board({ xIsNext, squares, onPlay }) {
    	function handleClick(i) {
    		if (calculateWinner(squares) || squares[i]) {
    			return;
    		}
    		const nextSquares = squares.slice();
    		if (xIsNext) {
    			nextSquares[i] = 'X';
    		} else {
    			nextSquares[i] = 'O';
    		}
    		onPlay(nextSquares);
    	}

    	const winner = calculateWinner(squares);
    	let status;
    	if (winner) {
    		status = 'Winner: ' + winner;
    	} else {
    		status = 'Next player: ' + (xIsNext ? 'X' : 'O');
    	}

    	return (
    		<>
    			<div className="status">{status}</div>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }

    export default function Game() {
    	const [history, setHistory] = useState([
    		Array(9).fill(null),
    	]);
    	const [currentMove, setCurrentMove] = useState(0);
    	const xIsNext = currentMove % 2 === 0;
    	const currentSquares = history[currentMove];

    	function handlePlay(nextSquares) {
    		const nextHistory = [
    			...history.slice(0, currentMove + 1),
    			nextSquares,
    		];
    		setHistory(nextHistory);
    		setCurrentMove(nextHistory.length - 1);
    	}

    	function jumpTo(nextMove) {
    		setCurrentMove(nextMove);
    	}

    	const moves = history.map((squares, move) => {
    		let description;
    		if (move > 0) {
    			description = 'Go to move #' + move;
    		} else {
    			description = 'Go to game start';
    		}
    		return (
    			<li key={move}>
    				<button onClick={() => jumpTo(move)}>
    					{description}
    				</button>
    			</li>
    		);
    	});

    	return (
    		<div className="game">
    			<div className="game-board">
    				<Board
    					xIsNext={xIsNext}
    					squares={currentSquares}
    					onPlay={handlePlay}
    				/>
    			</div>
    			<div className="game-info">
    				<ol>{moves}</ol>
    			</div>
    		</div>
    	);
    }

    function calculateWinner(squares) {
    	const lines = [
    		[0, 1, 2],
    		[3, 4, 5],
    		[6, 7, 8],
    		[0, 3, 6],
    		[1, 4, 7],
    		[2, 5, 8],
    		[0, 4, 8],
    		[2, 4, 6],
    	];
    	for (let i = 0; i < lines.length; i++) {
    		const [a, b, c] = lines[i];
    		if (
    			squares[a] &&
    			squares[a] === squares[b] &&
    			squares[a] === squares[c]
    		) {
    			return squares[a];
    		}
    	}
    	return null;
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/q2z497?view=Editor+%2B+Preview&module=%2Fsrc%2Findex.js" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Если код пока не имеет для вас смысла, или если вы не знакомы с синтаксисом кода, не волнуйтесь! Цель этого урока — помочь вам понять React и его синтаксис.

Мы рекомендуем вам ознакомиться с приведенной выше игрой "Крестики-нолики", прежде чем продолжить обучение. Одна из особенностей, которую вы заметите, это наличие пронумерованного списка справа от игрового поля. Этот список дает вам историю всех ходов, которые произошли в игре, и обновляется по мере ее прохождения.

После того как вы поиграете в готовую игру "Крестики-нолики", продолжайте прокручивать страницу. В этом уроке вы начнете с более простого шаблона. Следующим шагом мы подготовим вас к тому, чтобы вы могли начать создавать игру.

## Настройка для учебника {#setup-for-the-tutorial}

В живом редакторе кода ниже нажмите **Fork** в правом верхнем углу, чтобы открыть редактор в новой вкладке с помощью сайта CodeSandbox. CodeSandbox позволяет писать код в браузере и просматривать, как пользователи увидят созданное вами приложение. На новой вкладке должен появиться пустой квадрат и начальный код для этого учебника.

=== "App.js"

    ```js
    export default function Square() {
    	return <button className="square">X</button>;
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/j67kkz?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

!!!note ""

    Вы также можете следовать этому руководству, используя локальную среду разработки. Для этого вам необходимо:

    1.  Установить [Node.js](https://nodejs.org/en/)
    2.  На вкладке CodeSandbox, которую вы открыли ранее, нажмите кнопку в левом верхнем углу, чтобы открыть меню, а затем выберите **Файл --> Экспорт в ZIP** в этом меню, чтобы загрузить архив файлов локально.
    3.  Распакуйте архив, затем откройте терминал и `cd` в каталог, который вы распаковали.
    4.  Установите зависимости с помощью `npm install`.
    5.  Запустите `npm start` для запуска локального сервера и следуйте подсказкам для просмотра кода в браузере.

    Если вы застряли, не позволяйте этому остановить вас! Вместо этого следуйте указаниям онлайн и попробуйте локальную установку позже.

## Обзор {#overview}

Теперь, когда все готово, давайте сделаем обзор React!

### Проверка стартового кода {#inspecting-the-starter-code}

В CodeSandbox вы увидите три основных раздела:

![CodeSandbox со стартовым кодом](react-starter-code-codesandbox.png)

1.  Раздел _Files_ со списком файлов, таких как `App.js`, `index.js`, `styles.css` и папка `public`.
2.  Редактор _кода_, где вы увидите исходный код выбранного вами файла
3.  Раздел _браузер_, где вы увидите, как будет отображаться написанный вами код.

В разделе _Файлы_ должен быть выбран файл `App.js`. Содержимое этого файла в _редакторе кода_ должно быть таким:

```js
export default function Square() {
    return <button className="square">X</button>;
}
```

В разделе _браузер_ должен отображаться квадрат с буквой X, как показано ниже:

![x-filled square](x-filled-square.png)

Теперь давайте посмотрим на файлы в начальном коде.

#### `App.js`

Код в `App.js` создает _компонент_. В React компонент — это часть многократно используемого кода, который представляет собой часть пользовательского интерфейса. Компоненты используются для рендеринга, управления и обновления элементов пользовательского интерфейса в вашем приложении. Давайте рассмотрим компонент построчно, чтобы понять, что происходит:

```js hl_lines="1"
export default function Square() {
    return <button className="square">X</button>;
}
```

Первая строка определяет функцию `Square`. Ключевое слово JavaScript `export` делает эту функцию доступной за пределами данного файла. Ключевое слово `default` сообщает другим файлам, использующим ваш код, что это главная функция в вашем файле.

```js hl_lines="2"
export default function Square() {
    return <button className="square">X</button>;
}
```

Вторая строка возвращает кнопку. Ключевое слово JavaScript `return` означает, что все, что идет после него, возвращается в качестве значения вызывающей функции. `<button>` представляет собой _JSX-элемент_. JSX-элемент — это комбинация кода JavaScript и HTML-тегов, которая описывает то, что вы хотите отобразить. `className="square"` является свойством кнопки или _prop_, которое указывает CSS, как стилизовать кнопку. `X` является текстом, отображаемым внутри кнопки, а `</button>` закрывает JSX-элемент, указывая, что следующее содержимое не должно быть размещено внутри кнопки.

#### `styles.css`

Щелкните на файле с надписью `styles.css` в разделе _Files_ CodeSandbox. Этот файл определяет стили для вашего приложения React. Первые два _CSS-селектора_ (`*` и `body`) определяют стиль больших частей вашего приложения, а селектор `.square` определяет стиль любого компонента, у которого свойство `className` установлено в `square`. В вашем коде это будет соответствовать кнопке компонента Square в файле `App.js`.

#### `index.js`

Щелкните на файле с надписью `index.js` в разделе _Files_ CodeSandbox. Вы не будете редактировать этот файл во время урока, но он является связующим звеном между компонентом, который вы создали в файле `App.js`, и веб-браузером.

```js
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import './styles.css';

import App from './App';
```

Строки 1-5 объединяют все необходимые части вместе:

-   React
-   библиотека React для общения с веб-браузерами (React DOM)
-   стили для ваших компонентов
-   компонент, который вы создали в `App.js`.

Оставшаяся часть файла собирает все части вместе и вставляет конечный продукт в `index.html` в папке `public`.

### Создание доски {#building-the-board}

Давайте вернемся к `App.js`. Здесь вы проведете всю оставшуюся часть урока.

В настоящее время доска состоит только из одного квадрата, но вам нужно девять! Если вы просто попытаетесь скопировать и вставить свой квадрат, чтобы сделать два квадрата, как здесь:

```js hl_lines="3-4"
export default function Square() {
    return (
		<button className="square">X</button>
		<button className="square">X</button>
    );
}
```

Вы получите эту ошибку:

!!!danger "Console"

    <pre style="white-space: normal;">
    /src/App.js: Adjacent JSX elements must be wrapped in an enclosing tag. Did you want a JSX Fragment &lt;>...&lt;/>?
    </pre>

Компоненты React должны возвращать один JSX-элемент, а не несколько смежных JSX-элементов, как две кнопки. Чтобы исправить это, вы можете использовать _фрагменты_ для обертывания нескольких соседних JSX-элементов, как это сделано здесь:

```js hl_lines="3-6"
export default function Square() {
    return (
        <>
            <button className="square">X</button>
            <button className="square">X</button>
        </>
    );
}
```

Теперь вы должны увидеть:

![два x-заполненных квадрата](two-x-filled-squares.png)

Отлично! Теперь вам нужно просто скопировать-вставить несколько раз, чтобы добавить девять квадратов и...

![девять x-заполненных квадратов в линии](nine-x-filled-squares.png)

О нет! Все квадраты находятся в одной линии, а не в сетке, как нужно для нашей доски. Чтобы исправить это, вам нужно сгруппировать квадраты в ряды с помощью `div` и добавить несколько классов CSS. Пока вы это делаете, присвойте каждому квадрату номер, чтобы вы знали, где отображается каждый квадрат.

В файле `App.js` обновите компонент `Square`, чтобы он выглядел следующим образом:

```js hl_lines="3-19"
export default function Square() {
    return (
        <>
            <div className="board-row">
                <button className="square">1</button>
                <button className="square">2</button>
                <button className="square">3</button>
            </div>
            <div className="board-row">
                <button className="square">4</button>
                <button className="square">5</button>
                <button className="square">6</button>
            </div>
            <div className="board-row">
                <button className="square">7</button>
                <button className="square">8</button>
                <button className="square">9</button>
            </div>
        </>
    );
}
```

CSS, определенный в `styles.css`, стилизует div'ы с `className` в `board-row`. Теперь, когда вы сгруппировали свои компоненты в ряды с помощью стилизованных `div`, у вас есть доска для игры в крестики-нолики:

![доска крестики-нолики, заполненная цифрами от 1 до 9](number-filled-board.png)

Но теперь у вас возникла проблема. Ваш компонент, названный `Square`, на самом деле больше не является квадратом. Давайте исправим это, изменив название на `Board`:

```js hl_lines="1"
export default function Board() {
    //...
}
```

На этом этапе ваш код должен выглядеть примерно так:

=== "App.js"

    ```js
    export default function Board() {
    	return (
    		<>
    			<div className="board-row">
    				<button className="square">1</button>
    				<button className="square">2</button>
    				<button className="square">3</button>
    			</div>
    			<div className="board-row">
    				<button className="square">4</button>
    				<button className="square">5</button>
    				<button className="square">6</button>
    			</div>
    			<div className="board-row">
    				<button className="square">7</button>
    				<button className="square">8</button>
    				<button className="square">9</button>
    			</div>
    		</>
    	);
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/4n6z76?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

!!!note ""

    Это очень много для набора текста! Можно копировать и вставлять код с этой страницы. Однако, если вы хотите немного потрудиться, мы рекомендуем копировать только тот код, который вы сами набирали вручную хотя бы один раз.

### Передача данных через пропсы {#passing-data-through-props}

Далее вам нужно изменить значение квадрата с пустого на "X", когда пользователь нажимает на квадрат. Учитывая то, как вы построили доску, вам пришлось бы копировать код, обновляющий квадрат, девять раз (по одному разу для каждого квадрата)! Вместо копирования-вставки компонентная архитектура React позволяет создавать многократно используемые компоненты, чтобы избежать беспорядочного, дублирующегося кода.

Сначала скопируйте строку, определяющую ваш первый квадрат (`<button className="square">1</button>`) из компонента `Board` в новый компонент `Square`:

```js hl_lines="1-3"
function Square() {
    return <button className="square">1</button>;
}

export default function Board() {
    // ...
}
```

Затем вы обновите компонент Board для рендеринга этого компонента `Square`, используя синтаксис JSX:

```js hl_lines="5-19"
// ...
export default function Board() {
    return (
        <>
            <div className="board-row">
                <Square />
                <Square />
                <Square />
            </div>
            <div className="board-row">
                <Square />
                <Square />
                <Square />
            </div>
            <div className="board-row">
                <Square />
                <Square />
                <Square />
            </div>
        </>
    );
}
```

Обратите внимание, что в отличие от браузерных `div`, ваши собственные компоненты `Board` и `Square` должны начинаться с заглавной буквы.

Давайте посмотрим:

![доска с одним заполнением](board-filled-with-ones.png)

О нет! Вы потеряли пронумерованные квадраты, которые у вас были раньше. Теперь на каждом квадрате написано "1". Чтобы исправить это, вы будете использовать _props_ для передачи значения каждого квадрата от родительского компонента (`Board`) к его дочернему компоненту (`Square`).

Обновите компонент `Square`, чтобы он читал свойство `value`, которое вы передадите из `Board`:

```js hl_lines="1"
function Square({ value }) {
    return <button className="square">1</button>;
}
```

`function Square({ value })` указывает, что компоненту Square может быть передан параметр `value`.

Теперь вы хотите отображать это `value` вместо `1` внутри каждого квадрата. Попробуйте сделать это следующим образом:

```js hl_lines="2"
function Square({ value }) {
    return <button className="square">value</button>;
}
```

Упс, это не то, что вы хотели:

![доска, заполненная значением](board-filled-with-value.png)

Вы хотели вывести переменную JavaScript с именем `value` из вашего компонента, а не слово "value". Чтобы "перевести в JavaScript" из JSX, вам нужны фигурные скобки. Добавьте фигурные скобки вокруг `value` в JSX следующим образом:

```js hl_lines="2"
function Square({ value }) {
    return <button className="square">{value}</button>;
}
```

Пока что вы должны видеть пустую доску:

![пустая доска](empty-board.png)

Это происходит потому, что компонент `Board` еще не передал свойство `value` каждому компоненту `Square`, который он рендерит. Чтобы исправить это, добавьте свойство `value` в каждый компонент `Square`, отображаемый компонентом `Board`:

```js hl_lines="5-7 10-12 15-17"
export default function Board() {
    return (
        <>
            <div className="board-row">
                <Square value="1" />
                <Square value="2" />
                <Square value="3" />
            </div>
            <div className="board-row">
                <Square value="4" />
                <Square value="5" />
                <Square value="6" />
            </div>
            <div className="board-row">
                <Square value="7" />
                <Square value="8" />
                <Square value="9" />
            </div>
        </>
    );
}
```

Теперь вы снова должны увидеть сетку из цифр:

![доска крестики-нолики, заполненная числами от 1 до 9](number-filled-board.png)

Ваш обновленный код должен выглядеть следующим образом:

=== "App.js"

    ```js
    function Square({ value }) {
    	return <button className="square">{value}</button>;
    }

    export default function Board() {
    	return (
    		<>
    			<div className="board-row">
    				<Square value="1" />
    				<Square value="2" />
    				<Square value="3" />
    			</div>
    			<div className="board-row">
    				<Square value="4" />
    				<Square value="5" />
    				<Square value="6" />
    			</div>
    			<div className="board-row">
    				<Square value="7" />
    				<Square value="8" />
    				<Square value="9" />
    			</div>
    		</>
    	);
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/hnfrzc?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

### Создание интерактивного компонента {#making-an-interactive-component}

Давайте заполним компонент `Square` символом `X` при нажатии на него. Объявите функцию `handleClick` внутри `Square`. Затем добавьте `onClick` в props JSX-элемента button, возвращаемого из `Square`:

```js hl_lines="2-4 7"
function Square({ value }) {
    function handleClick() {
        console.log('нажато!');
    }

    return (
        <button className="square" onClick={handleClick}>
            {value}
        </button>
    );
}
```

Если вы сейчас щелкните на квадрате, вы увидите лог с надписью `нажато!` во вкладке _Консоль_ внизу раздела _Браузер_ в CodeSandbox. Если щелкнуть по квадрату более одного раза, то в журнале снова появится сообщение `нажато!`. Повторяющиеся консольные журналы с одним и тем же сообщением не будут создавать больше строк в консоли. Вместо этого вы увидите увеличивающийся счетчик рядом с первым журналом `нажато!`.

!!!note ""

    Если вы следуете этому руководству, используя локальную среду разработки, вам нужно открыть консоль вашего браузера. Например, если вы используете браузер Chrome, вы можете открыть консоль с помощью сочетания клавиш ++shift+ctrl+j++ (в Windows/Linux) или ++option+command+j++ (в macOS).

Следующим шагом вы хотите, чтобы компонент `Square` "запомнил", что на него нажали, и заполнил его меткой "X". Для "запоминания" компонент использует _state_.

React предоставляет специальную функцию `useState`, которую вы можете вызвать из вашего компонента, чтобы позволить ему "запомнить" что-то. Давайте сохраним текущее значение `Square` в state и изменим его при нажатии на `Square`.

Импортируйте `useState` в верхней части файла. Удалите свойство `value` из компонента `Square`. Вместо этого добавьте новую строку в начало `Square`, которая вызывает `useState`. Пусть она возвращает переменную состояния с именем `value`:

```js hl_lines="1 3-4"
import { useState } from 'react';

function Square() {
    const [value, setValue] = useState(null);

    function handleClick() {
        //...
    }
}
```

`value` хранит значение, а `setValue` — это функция, которая может быть использована для изменения значения. Значение `null`, переданное в `useState`, используется в качестве начального значения для этой переменной состояния, поэтому `value` здесь начинается с `null`.

Поскольку компонент `Square` больше не принимает пропсы, вы удалите пропс `value` из всех девяти компонентов Square, созданных компонентом Board:

```js hl_lines="6-8 11-13 16-18"
// ...
export default function Board() {
    return (
        <>
            <div className="board-row">
                <Square />
                <Square />
                <Square />
            </div>
            <div className="board-row">
                <Square />
                <Square />
                <Square />
            </div>
            <div className="board-row">
                <Square />
                <Square />
                <Square />
            </div>
        </>
    );
}
```

Теперь вы измените `Square`, чтобы при нажатии на него отображался символ "X". Замените обработчик события `console.log("нажато!");` на `setValue('X');`. Теперь ваш компонент `Square` выглядит следующим образом:

```js hl_lines="5"
function Square() {
    const [value, setValue] = useState(null);

    function handleClick() {
        setValue('X');
    }

    return (
        <button className="square" onClick={handleClick}>
            {value}
        </button>
    );
}
```

Вызывая эту функцию `set` из обработчика `onClick`, вы говорите React перерисовать этот `Square` при каждом нажатии на его `<button>`. После обновления `value` квадрата будет равно `'X'`, поэтому вы увидите "X" на игровом поле. Нажмите на любой квадрат, и "X" должен появиться:

![добавление иксов на игровое поле](tictac-adding-x-s.gif)

Каждый квадрат имеет свое собственное состояние: `value`, хранящееся в каждом квадрате, полностью независимо от других. Когда вы вызываете функцию `set` в компоненте, React автоматически обновляет и дочерние компоненты внутри него.

После внесения вышеуказанных изменений ваш код будет выглядеть следующим образом:

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square() {
    	const [value, setValue] = useState(null);

    	function handleClick() {
    		setValue('X');
    	}

    	return (
    		<button className="square" onClick={handleClick}>
    			{value}
    		</button>
    	);
    }

    export default function Board() {
    	return (
    		<>
    			<div className="board-row">
    				<Square />
    				<Square />
    				<Square />
    			</div>
    			<div className="board-row">
    				<Square />
    				<Square />
    				<Square />
    			</div>
    			<div className="board-row">
    				<Square />
    				<Square />
    				<Square />
    			</div>
    		</>
    	);
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/9tcmyz?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

### React Developer Tools {#react-developer-tools}

React DevTools позволяет вам проверять пропсы и состояние ваших компонентов React. Вы можете найти вкладку React DevTools в нижней части раздела _browser_ в CodeSandbox:

![React DevTools в CodeSandbox](codesandbox-devtools.png)

Чтобы изучить определенный компонент на экране, используйте кнопку в левом верхнем углу React DevTools:

![Выбор компонентов на странице с помощью React DevTools](devtools-select.gif)

!!!note "Расширение для браузеров React DevTools"

    Для локальной разработки React DevTools доступен как расширение для браузеров [Chrome](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en), [Firefox](https://addons.mozilla.org/en-US/firefox/addon/react-devtools/) и [Edge](https://microsoftedge.microsoft.com/addons/detail/react-developer-tools/gpphkfbcpidddadnkolkpfckpihlkkil). Установите его, и в вашем браузере появится вкладка _Компоненты_ Инструменты разработчика для сайтов, использующих React.

## Завершение игры {#completing-the-game}

К этому моменту у вас есть все основные компоненты для игры в крестики-нолики. Чтобы игра была полноценной, теперь вам нужно чередовать размещение "X" и "O" на доске, и вам нужен способ определения победителя.

### Поднятие состояния {#lifting-state-up}

В настоящее время каждый компонент `Square` хранит часть состояния игры. Чтобы проверить победителя в игре "крестики-нолики", `Board` должен каким-то образом узнать состояние каждого из 9 компонентов `Square`.

Как бы вы к этому подошли? Сначала вы можете предположить, что `Board` должен "спросить" у каждого `Square` о состоянии этого `Square`. Хотя такой подход технически возможен в React, мы не рекомендуем его использовать, поскольку код становится сложным для понимания, подверженным ошибкам и трудно поддающимся рефакторингу. Вместо этого лучшим подходом является хранение состояния игры в родительском компоненте `Board`, а не в каждом `Square`. Компонент `Board` может указать каждому `Square`, что отображать, передав пропс, как вы делали, когда передавали число каждому квадрату.

**Чтобы собрать данные от нескольких дочерних компонентов или чтобы два дочерних компонента общались друг с другом, объявите общее состояние в их родительском компоненте. Родительский компонент может передавать это состояние дочерним компонентам через пропсы. Таким образом, дочерние компоненты синхронизируются друг с другом и со своим родителем**.

Передача состояния в родительский компонент — обычное дело при рефакторинге компонентов React.

Давайте воспользуемся этой возможностью и попробуем это сделать. Отредактируйте компонент `Board` так, чтобы он объявил переменную состояния с именем `squares`, которая по умолчанию содержит массив из 9 нулей, соответствующих 9 квадратам:

```js hl_lines="3"
// ...
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));
  return (
    // ...
  );
}
```

`Array(9).fill(null)` создает массив с девятью элементами и устанавливает каждый из них в `null`. Вызов `useState()` вокруг него объявляет переменную состояния `squares`, которая первоначально устанавливается в этот массив. Каждая запись в массиве соответствует значению квадрата. Когда вы позже заполните доску, массив `squares` будет выглядеть следующим образом:

```js
['O', null, 'X', 'X', 'X', 'O', 'O', null, null];
```

Теперь ваш компонент `Board` должен передать свойство `value` каждому `Square`, который он отображает:

```js hl_lines="8-10 13-15 18-20"
export default function Board() {
    const [squares, setSquares] = useState(
        Array(9).fill(null)
    );
    return (
        <>
            <div className="board-row">
                <Square value={squares[0]} />
                <Square value={squares[1]} />
                <Square value={squares[2]} />
            </div>
            <div className="board-row">
                <Square value={squares[3]} />
                <Square value={squares[4]} />
                <Square value={squares[5]} />
            </div>
            <div className="board-row">
                <Square value={squares[6]} />
                <Square value={squares[7]} />
                <Square value={squares[8]} />
            </div>
        </>
    );
}
```

Далее вы отредактируете компонент `Square`, чтобы он получал свойство `value` от компонента `Board`. Это потребует удаления собственного отслеживания состояния `value` компонента `Square` и пропса `onClick` кнопки:

```js hl_lines="1-2"
function Square({ value }) {
    return <button className="square">{value}</button>;
}
```

В этот момент вы должны увидеть пустую доску для игры в крестики-нолики:

![пустая доска](empty-board.png)

А ваш код должен выглядеть следующим образом:

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value }) {
    	return <button className="square">{value}</button>;
    }

    export default function Board() {
    	const [squares, setSquares] = useState(
    		Array(9).fill(null)
    	);
    	return (
    		<>
    			<div className="board-row">
    				<Square value={squares[0]} />
    				<Square value={squares[1]} />
    				<Square value={squares[2]} />
    			</div>
    			<div className="board-row">
    				<Square value={squares[3]} />
    				<Square value={squares[4]} />
    				<Square value={squares[5]} />
    			</div>
    			<div className="board-row">
    				<Square value={squares[6]} />
    				<Square value={squares[7]} />
    				<Square value={squares[8]} />
    			</div>
    		</>
    	);
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/84sswn?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Теперь каждый квадрат будет получать свойство `value`, которое будет либо `'X'`, `'O'`, либо `null` для пустых квадратов.

Далее необходимо изменить то, что происходит при нажатии на `Square`. Компонент `Board` теперь определяет, какие квадраты заполнены. Вам нужно создать способ для `Square` обновить состояние `Board`. Поскольку состояние является частным для компонента, который его определяет, вы не можете обновлять состояние `Board` непосредственно из `Square`.

Вместо этого вы передадите функцию от компонента `Board` компоненту `Square`, а `Square` будет вызывать эту функцию при нажатии на квадрат. Начнем с функции, которую компонент `Square` будет вызывать при нажатии на него. Вы назовете эту функцию `onSquareClick`:

```js hl_lines="3"
function Square({ value }) {
    return (
        <button className="square" onClick={onSquareClick}>
            {value}
        </button>
    );
}
```

Далее вы добавите функцию `onSquareClick` в пропс компонента `Square`:

```js hl_lines="1"
function Square({ value, onSquareClick }) {
    return (
        <button className="square" onClick={onSquareClick}>
            {value}
        </button>
    );
}
```

Теперь вы подключите пропс `onSquareClick` к функции в компоненте `Board`, которую назовете `handleClick`. Чтобы связать `onSquareClick` с `handleClick`, вы передадите функцию в пропс `onSquareClick` первого компонента `Square`:

```js hl_lines="11"
export default function Board() {
    const [squares, setSquares] = useState(
        Array(9).fill(null)
    );

    return (
        <>
            <div className="board-row">
                <Square
                    value={squares[0]}
                    onSquareClick={handleClick}
                />
                //...
            </div>
        </>
    );
}
```

Наконец, вы определите функцию `handleClick` внутри компонента `Board`, чтобы обновить массив `squares`, содержащий состояние вашей доски:

```js hl_lines="4-8"
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick() {
    const nextSquares = squares.slice();
    nextSquares[0] = "X";
    setSquares(nextSquares);
  }

  return (
    // ...
  )
}
```

Функция `handleClick` создает копию массива `squares` (`nextSquares`) с помощью метода массива JavaScript `slice()`. Затем функция `handleClick` обновляет массив `nextSquares`, добавляя `X` в первый (индекс `[0]`) квадрат.

Вызов функции `setSquares` позволяет React узнать, что состояние компонента изменилось. Это вызовет перерисовку компонентов, использующих состояние `squares` (`Board`), а также его дочерних компонентов (компоненты `Square`, составляющие доску).

!!!note "Замыкания"

    JavaScript поддерживает [Замыкания](https://developer.mozilla.org/ru/docs/Web/JavaScript/Closures), что означает, что внутренняя функция (например, `handleClick`) имеет доступ к переменным и функциям, определенным во внешней функции (например, `Board`). Функция `handleClick` может читать состояние `squares` и вызывать метод `setSquares`, потому что они оба определены внутри функции `Board`.

Теперь вы можете добавить крестики на доску... но только в верхний левый квадрат. Ваша функция `handleClick` жестко закодирована для обновления индекса левого верхнего квадрата (`0`). Давайте обновим `handleClick`, чтобы она могла обновлять любой квадрат. Добавьте аргумент `i` к функции `handleClick`, который принимает индекс квадрата, который нужно обновить:

```js hl_lines="4 6"
export default function Board() {
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    const nextSquares = squares.slice();
    nextSquares[i] = "X";
    setSquares(nextSquares);
  }

  return (
    // ...
  )
}
```

Далее вам нужно передать `i` в `handleClick`. Вы можете попытаться установить свойство `onSquareClick` для квадрата как `handleClick(0)` непосредственно в JSX, как это сделано здесь, но это не сработает:

```js
<Square value={squares[0]} onSquareClick={handleClick(0)} />
```

Вот почему это не работает. Вызов `handleClick(0)` будет частью рендеринга компонента доски. Поскольку `handleClick(0)` изменяет состояние компонента доски, вызывая `setSquares`, весь ваш компонент доски будет отрисован заново. Но при этом снова запускается `handleClick(0)`, что приводит к бесконечному циклу:

!!!danger "Console"

    <pre style="white-space: normal">Too many re-renders. React limits the number of renders to prevent an infinite loop.</pre>

Почему эта проблема не возникала раньше?

Когда вы передавали `onSquareClick={handleClick}`, вы передавали функцию `handleClick` как пропс. Вы не вызывали ее! Но теперь вы _вызываете_ эту функцию сразу — обратите внимание на скобки в `handleClick(0)` — и поэтому она запускается слишком рано. Вы не _хотите_ вызывать `handleClick`, пока пользователь не нажмет кнопку!

Это можно исправить, создав функцию типа `handleFirstSquareClick`, которая вызывает `handleClick(0)`, функцию типа `handleSecondSquareClick`, которая вызывает `handleClick(1)`, и так далее. Вы должны передавать (а не вызывать) эти функции в качестве пропсов, например `onSquareClick={handleFirstSquareClick}`. Это решит проблему бесконечного цикла.

Однако определять девять различных функций и давать каждой из них имя слишком многословно. Вместо этого давайте сделаем следующее:

```js hl_lines="8"
export default function Board() {
    // ...
    return (
        <>
            <div className="board-row">
                <Square
                    value={squares[0]}
                    onSquareClick={() => handleClick(0)}
                />
                // ...
            </div>
        </>
    );
}
```

Обратите внимание на новый синтаксис `() =>`. Здесь `() => handleClick(0)` — это _стрелочная функция,_ что является более коротким способом определения функций. Когда квадрат будет щелкнут, код после `=>` "стрелки" будет запущен, вызывая `handleClick(0)`.

Теперь вам нужно обновить остальные восемь квадратов, чтобы они вызывали `handleClick` из переданных вам функций стрелок. Убедитесь, что аргумент для каждого вызова `handleClick` соответствует индексу правильного квадрата:

```js hl_lines="6-17 20-31 34-45"
export default function Board() {
    // ...
    return (
        <>
            <div className="board-row">
                <Square
                    value={squares[0]}
                    onSquareClick={() => handleClick(0)}
                />
                <Square
                    value={squares[1]}
                    onSquareClick={() => handleClick(1)}
                />
                <Square
                    value={squares[2]}
                    onSquareClick={() => handleClick(2)}
                />
            </div>
            <div className="board-row">
                <Square
                    value={squares[3]}
                    onSquareClick={() => handleClick(3)}
                />
                <Square
                    value={squares[4]}
                    onSquareClick={() => handleClick(4)}
                />
                <Square
                    value={squares[5]}
                    onSquareClick={() => handleClick(5)}
                />
            </div>
            <div className="board-row">
                <Square
                    value={squares[6]}
                    onSquareClick={() => handleClick(6)}
                />
                <Square
                    value={squares[7]}
                    onSquareClick={() => handleClick(7)}
                />
                <Square
                    value={squares[8]}
                    onSquareClick={() => handleClick(8)}
                />
            </div>
        </>
    );
}
```

Теперь вы снова можете добавлять иксы к любому квадрату на доске, щелкая по ним:

![заполнение доски X](tictac-adding-x-s.gif)

Но на этот раз все управление состоянием осуществляется компонентом `Board`!

Вот как должен выглядеть ваш код:

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    export default function Board() {
    	const [squares, setSquares] = useState(
    		Array(9).fill(null)
    	);

    	function handleClick(i) {
    		const nextSquares = squares.slice();
    		nextSquares[i] = 'X';
    		setSquares(nextSquares);
    	}

    	return (
    		<>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/52z5dj?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Теперь, когда обработка состояния находится в компоненте `Board`, родительский компонент `Board` передает пропсы дочерним компонентам `Square`, чтобы они отображались правильно. При нажатии на `Square` дочерний компонент `Square` теперь просит родительский компонент `Board` обновить состояние доски. Когда состояние `Board` изменяется, компонент `Board` и каждый дочерний `Square` автоматически перерисовываются. Сохранение состояния всех квадратов в компоненте `Board` позволит ему в будущем определить победителя.

Давайте вспомним, что происходит, когда пользователь нажимает на левый верхний квадрат на вашей доске, чтобы добавить к нему `X`:

1.  Щелчок на левом верхнем квадрате запускает функцию, которую `button` получила как свой пропс `onClick` от `Square`. Компонент `Square` получил эту функцию как свой пропс `onSquareClick` от `Board`. Компонент `Board` определил эту функцию непосредственно в JSX. Она вызывает `handleClick` с аргументом `0`.
2.  `handleClick` использует аргумент (`0`) для обновления первого элемента массива `squares` с `null` до `X`.
3.  Состояние `squares` компонента `Board` было обновлено, поэтому `Board` и все его дочерние элементы перерисовываются. Это приводит к тому, что свойство `value` компонента `Square` с индексом `0` изменяется с `null` на `X`.

В итоге пользователь видит, что левый верхний квадрат после щелчка превратился из пустого в квадрат с символом `X`.

!!!note "Соглашения по именованию обработчиков"

    Атрибут `onClick` элемента DOM `<button>` имеет особое значение для React, поскольку это встроенный компонент. Для пользовательских компонентов, таких как `Square`, именование зависит от вас. Вы можете дать любое имя атрибуту `onSquareClick` элемента `Square` или функции `handleClick` элемента `Board`, и код будет работать одинаково. В React принято использовать имена `onSomething` для пропсов, которые представляют события, и `handleSomething` для определений функций, которые обрабатывают эти события.

### Почему важна иммутабельность {#why-immutability-is-important}

Обратите внимание, как в `handleClick` вы вызываете `.slice()` для создания копии массива `squares` вместо того, чтобы модифицировать существующий массив. Чтобы объяснить почему, нам нужно обсудить неизменяемость и почему неизменяемость важна для изучения.

Обычно существует два подхода к изменению данных. Первый подход заключается в _изменении_ данных путем прямого изменения их значений. Второй подход заключается в замене данных новой копией, содержащей желаемые изменения. Вот как будет выглядеть изменение массива `squares`:

```js
const squares = [
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
];
squares[0] = 'X';
// Now `squares` is ["X", null, null, null, null, null, null, null, null];
```

А вот как это будет выглядеть, если вы измените данные, не изменяя массив `squares`:

```js
const squares = [
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
];
const nextSquares = [
    'X',
    null,
    null,
    null,
    null,
    null,
    null,
    null,
    null,
];
// Now `squares` is unchanged, but `nextSquares`
// first element is 'X' rather than `null`
```

Результат один и тот же, но, не изменяя непосредственно данные, вы получаете несколько преимуществ.

Неизменяемость значительно упрощает реализацию сложных функций. Позже в этом учебнике вы реализуете функцию "путешествия во времени", которая позволит вам просматривать историю игры и "прыгать назад" к прошлым ходам. Эта функциональность не является специфической для игр — возможность отмены и повторения определенных действий является обычным требованием для приложений. Избегая прямой мутации данных, вы можете сохранить предыдущие версии данных и использовать их в дальнейшем.

Есть и еще одно преимущество неизменяемости. По умолчанию все дочерние компоненты автоматически перерисовываются при изменении состояния родительского компонента. Это касается даже тех дочерних компонентов, которые не были затронуты изменением. Хотя сам по себе повторный рендеринг незаметен для пользователя (вы не должны активно пытаться избежать его!), вы можете захотеть пропустить повторный рендеринг той части дерева, которая явно не была затронута им по соображениям производительности. Неизменность делает очень дешевым для компонентов сравнение того, изменились их данные или нет. Вы можете узнать больше о том, как React выбирает время для повторного отображения компонента в [справочнике API `memo`](../reference/react/memo.md).

### Ходы {#taking-turns}

Настало время исправить главный недостаток этой игры в крестики-нолики: буквы "О" не могут быть отмечены на доске.

По умолчанию первым ходом будет "X". Давайте проследим за этим, добавив еще один элемент состояния в компонент `Board`:

```js hl_lines="2"
function Board() {
    const [xIsNext, setXIsNext] = useState(true);
    const [squares, setSquares] = useState(
        Array(9).fill(null)
    );

    // ...
}
```

Каждый раз, когда игрок перемещается, `xIsNext` (булево значение) будет перевернуто, чтобы определить, какой игрок идет следующим, и состояние игры будет сохранено. Вы обновите функцию `Board` `handleClick`, чтобы перевернуть значение `xIsNext`:

```js hl_lines="7-11 13"
export default function Board() {
  const [xIsNext, setXIsNext] = useState(true);
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = "X";
    } else {
      nextSquares[i] = "O";
    }
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  return (
    //...
  );
}
```

Теперь, когда вы нажимаете на разные квадраты, они будут чередоваться между `X` и `O`, как и должно быть!

Но подождите, есть проблема. Попробуйте нажать на один и тот же квадрат несколько раз:

![O перезаписывает X](o-replaces-x.gif)

X заменяется на O! Хотя это добавило бы очень интересный поворот в игру, мы пока будем придерживаться оригинальных правил.

Когда вы помечаете квадрат символом `X` или `O`, вы сначала не проверяете, не имеет ли квадрат уже значения `X` или `O`. Вы можете исправить это, _возвращаясь раньше_. Вы будете проверять, не имеет ли квадрат уже значение `X` или `O`. Если квадрат уже заполнен, вы `return` в функцию `handleClick` раньше, чем она попытается обновить состояние доски.

```js hl_lines="2-4"
function handleClick(i) {
    if (squares[i]) {
        return;
    }
    const nextSquares = squares.slice();
    //...
}
```

Теперь вы можете добавлять `X` или `O` только в пустые квадраты! Вот как должен выглядеть ваш код на этом этапе:

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    export default function Board() {
    	const [xIsNext, setXIsNext] = useState(true);
    	const [squares, setSquares] = useState(
    		Array(9).fill(null)
    	);

    	function handleClick(i) {
    		if (squares[i]) {
    			return;
    		}
    		const nextSquares = squares.slice();
    		if (xIsNext) {
    			nextSquares[i] = 'X';
    		} else {
    			nextSquares[i] = 'O';
    		}
    		setSquares(nextSquares);
    		setXIsNext(!xIsNext);
    	}

    	return (
    		<>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/p47pm8?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

### Объявление победителя {#declaring-a-winner}

Теперь, когда игроки могут ходить по очереди, вы захотите показать, когда игра выиграна и больше не осталось ходов. Для этого добавьте вспомогательную функцию `calculateWinner`, которая принимает массив из 9 квадратов, проверяет наличие победителя и возвращает `'X'`, `'O'` или `null` в зависимости от ситуации. Не стоит слишком беспокоиться о функции `calculateWinner`, она не является специфичной для React:

```js
export default function Board() {
    //...
}

function calculateWinner(squares) {
    const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
    ];
    for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
            squares[a] &&
            squares[a] === squares[b] &&
            squares[a] === squares[c]
        ) {
            return squares[a];
        }
    }
    return null;
}
```

!!!note ""

    Не имеет значения, определяете ли вы `calculateWinner` до или после `Board`. Давайте поместим его в конец, чтобы вам не приходилось прокручивать его мимо каждый раз, когда вы редактируете свои компоненты.

Вы будете вызывать `calculateWinner(squares)` в функции `handleClick` компонента `Board`, чтобы проверить, выиграл ли игрок. Вы можете выполнить эту проверку одновременно с проверкой того, нажал ли пользователь на квадрат, который уже имеет `X` или `O`. В обоих случаях мы хотели бы вернуть раннее значение:

```js hl_lines="2"
function handleClick(i) {
    if (squares[i] || calculateWinner(squares)) {
        return;
    }
    const nextSquares = squares.slice();
    //...
}
```

Чтобы сообщить игрокам об окончании игры, можно вывести на экран текст, например, "Победитель: X" или "Победитель: O". Для этого вы добавите секцию `status` в компонент `Board`. Статус будет отображать победителя, если игра закончилась, а если игра продолжается, то будет отображаться ход следующего игрока:

```js hl_lines="3-9 13"
export default function Board() {
  // ...
  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = "Winner: " + winner;
  } else {
    status = "Next player: " + (xIsNext ? "X" : "O");
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        // ...
  )
}
```

Поздравляю! Теперь у вас есть работающая игра в крестики-нолики. И вы только что изучили основы React. Так что _вы_ здесь настоящий победитель. Вот как должен выглядеть код:

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    export default function Board() {
    	const [xIsNext, setXIsNext] = useState(true);
    	const [squares, setSquares] = useState(
    		Array(9).fill(null)
    	);

    	function handleClick(i) {
    		if (calculateWinner(squares) || squares[i]) {
    			return;
    		}
    		const nextSquares = squares.slice();
    		if (xIsNext) {
    			nextSquares[i] = 'X';
    		} else {
    			nextSquares[i] = 'O';
    		}
    		setSquares(nextSquares);
    		setXIsNext(!xIsNext);
    	}

    	const winner = calculateWinner(squares);
    	let status;
    	if (winner) {
    		status = 'Winner: ' + winner;
    	} else {
    		status = 'Next player: ' + (xIsNext ? 'X' : 'O');
    	}

    	return (
    		<>
    			<div className="status">{status}</div>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }

    function calculateWinner(squares) {
    	const lines = [
    		[0, 1, 2],
    		[3, 4, 5],
    		[6, 7, 8],
    		[0, 3, 6],
    		[1, 4, 7],
    		[2, 5, 8],
    		[0, 4, 8],
    		[2, 4, 6],
    	];
    	for (let i = 0; i < lines.length; i++) {
    		const [a, b, c] = lines[i];
    		if (
    			squares[a] &&
    			squares[a] === squares[b] &&
    			squares[a] === squares[c]
    		) {
    			return squares[a];
    		}
    	}
    	return null;
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/6frjc5?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

## Добавление путешествия во времени {#adding-time-travel}

В качестве последнего упражнения давайте сделаем возможность "вернуться в прошлое" к предыдущим ходам в игре.

### Хранение истории ходов {#storing-a-history-of-moves}

Если бы вы мутировали массив `squares, то реализовать путешествие во времени было бы очень сложно.

Однако вы использовали `slice()` для создания новой копии массива `squares` после каждого хода и рассматривали его как неизменяемый. Это позволит вам хранить все прошлые версии массива `squares` и перемещаться между уже произошедшими ходами.

Вы будете хранить прошлые массивы `squares` в другом массиве под названием `history`, который вы будете хранить как новую переменную состояния. Массив `history` представляет все состояния доски, от первого до последнего хода, и имеет следующую форму:

```js
[
    // Before first move
    [null, null, null, null, null, null, null, null, null],
    // After first move
    [null, null, null, null, 'X', null, null, null, null],
    // After second move
    [null, null, null, null, 'X', null, null, null, 'O'],
    // ...
];
```

### Поднимаем состояние, снова {#lifting-state-up-again}

Теперь вы напишете новый компонент верхнего уровня под названием `Game` для отображения списка прошлых ходов. Именно здесь вы разместите состояние `history`, которое содержит всю историю игры.

Поместив состояние `history` в компонент `Game`, вы сможете удалить состояние `squares` из его дочернего компонента `Board`. Точно так же, как вы "подняли состояние" из компонента `Square` в компонент `Board`, теперь вы поднимете его из `Board` в компонент верхнего уровня `Game`. Это дает компоненту `Game` полный контроль над данными `Board` и позволяет ему инструктировать `Board` для отображения предыдущих ходов из `history`.

Сначала добавьте компонент `Game` с `export default`. Пусть он отобразит компонент `Board` и некоторую разметку:

```js hl_lines="1 5-16"
function Board() {
    // ...
}

export default function Game() {
    return (
        <div className="game">
            <div className="game-board">
                <Board />
            </div>
            <div className="game-info">
                <ol>{/*TODO*/}</ol>
            </div>
        </div>
    );
}
```

Обратите внимание, что вы удаляете ключевые слова `export default` перед объявлением `function Board() {` и добавляете их перед объявлением `function Game() {`. Это указывает вашему файлу `index.js` использовать компонент `Game` в качестве компонента верхнего уровня вместо компонента `Board`. Дополнительные `div`, возвращаемые компонентом `Game`, освобождают место для информации об игре, которую вы добавите на доску позже.

Добавьте некоторое состояние в компонент `Game`, чтобы отслеживать, какой игрок следующий и историю ходов:

```js hl_lines="2-5"
export default function Game() {
    const [xIsNext, setXIsNext] = useState(true);
    const [history, setHistory] = useState([
        Array(9).fill(null),
    ]);
    // ...
}
```

Обратите внимание, что `[Array(9).fill(null)]` — это массив с одним элементом, который сам является массивом из 9 `null`.

Чтобы отобразить квадраты для текущего хода, вам нужно прочитать массив последних квадратов из `history`. Вам не нужно `useState` для этого — у вас уже достаточно информации, чтобы вычислить ее во время рендеринга:

```js hl_lines="6"
export default function Game() {
    const [xIsNext, setXIsNext] = useState(true);
    const [history, setHistory] = useState([
        Array(9).fill(null),
    ]);
    const currentSquares = history[history.length - 1];
    // ...
}
```

Далее создайте функцию `handlePlay` внутри компонента `Game`, которая будет вызываться компонентом `Board` для обновления игры. Передайте `xIsNext`, `currentSquares` и `handlePlay` в качестве пропсов компоненту `Board`:

```js hl_lines="8-10 18"
export default function Game() {
    const [xIsNext, setXIsNext] = useState(true);
    const [history, setHistory] = useState([
        Array(9).fill(null),
    ]);
    const currentSquares = history[history.length - 1];

    function handlePlay(nextSquares) {
        // TODO
    }

    return (
        <div className="game">
            <div className="game-board">
                <Board
                    xIsNext={xIsNext}
                    squares={currentSquares}
                    onPlay={handlePlay}
                />
                {/* ... */}
            </div>
        </div>
    );
}
```

Давайте сделаем компонент `Board` полностью управляемым пропсами, которые он получает. Измените компонент `Board` так, чтобы он принимал три пропса: `xIsNext`, `squares` и новую функцию `onPlay`, которую `Board` может вызывать с обновленным массивом квадратов, когда игрок делает ход. Затем удалите первые две строки функции `Board`, которые вызывают `useState`:

```js hl_lines="1"
function Board({ xIsNext, squares, onPlay }) {
    function handleClick(i) {
        //...
    }
    // ...
}
```

Теперь замените вызовы `setSquares` и `setXIsNext` в `handleClick` в компоненте `Board` одним вызовом вашей новой функции `onPlay`, чтобы компонент `Game` мог обновлять `Board`, когда пользователь нажимает на квадрат:

```js hl_lines="12"
function Board({ xIsNext, squares, onPlay }) {
    function handleClick(i) {
        if (calculateWinner(squares) || squares[i]) {
            return;
        }
        const nextSquares = squares.slice();
        if (xIsNext) {
            nextSquares[i] = 'X';
        } else {
            nextSquares[i] = 'O';
        }
        onPlay(nextSquares);
    }
    //...
}
```

Компонент `Board` полностью управляется пропсами, передаваемыми ему компонентом `Game`. Вам необходимо реализовать функцию `handlePlay` в компоненте `Game`, чтобы игра снова заработала.

Что должна делать функция `handlePlay` при вызове? Помните, что раньше `Board` вызывал `setSquares` с обновленным массивом; теперь он передает обновленный массив `squares` в `onPlay`.

Функция `handlePlay` должна обновить состояние `Game`, чтобы вызвать повторный рендеринг, но у вас больше нет функции `setSquares`, которую вы можете вызвать — теперь вы используете переменную состояния `history` для хранения этой информации. Вы хотите обновить `history`, добавив обновленный массив `squares` в качестве новой записи истории. Вы также хотите переключить `xIsNext`, как это делал `Board`:

```js hl_lines="4-5"
export default function Game() {
    //...
    function handlePlay(nextSquares) {
        setHistory([...history, nextSquares]);
        setXIsNext(!xIsNext);
    }
    //...
}
```

Здесь `[...history, nextSquares]` создает новый массив, содержащий все элементы в `history`, а затем `nextSquares`. ([Spread-синтаксис `...history`](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Operators/Spread_syntax) можно прочитать как "перечислить все элементы в `history`").

Например, если `history` будет `[[null,null,null], ["X",null,null]]` и `nextSquares` будет `["X",null, "O"]`, то новый массив `[...history, nextSquares]` будет `[[null,null,null], ["X",null,null], ["X",null, "O"]]`.

На этом этапе вы перенесли состояние в компонент `Game`, и пользовательский интерфейс должен быть полностью рабочим, как и до рефакторинга. Вот как должен выглядеть код на этом этапе:

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    function Board({ xIsNext, squares, onPlay }) {
    	function handleClick(i) {
    		if (calculateWinner(squares) || squares[i]) {
    			return;
    		}
    		const nextSquares = squares.slice();
    		if (xIsNext) {
    			nextSquares[i] = 'X';
    		} else {
    			nextSquares[i] = 'O';
    		}
    		onPlay(nextSquares);
    	}

    	const winner = calculateWinner(squares);
    	let status;
    	if (winner) {
    		status = 'Winner: ' + winner;
    	} else {
    		status = 'Next player: ' + (xIsNext ? 'X' : 'O');
    	}

    	return (
    		<>
    			<div className="status">{status}</div>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }

    export default function Game() {
    	const [xIsNext, setXIsNext] = useState(true);
    	const [history, setHistory] = useState([
    		Array(9).fill(null),
    	]);
    	const currentSquares = history[history.length - 1];

    	function handlePlay(nextSquares) {
    		setHistory([...history, nextSquares]);
    		setXIsNext(!xIsNext);
    	}

    	return (
    		<div className="game">
    			<div className="game-board">
    				<Board
    					xIsNext={xIsNext}
    					squares={currentSquares}
    					onPlay={handlePlay}
    				/>
    			</div>
    			<div className="game-info">
    				<ol>{/*TODO*/}</ol>
    			</div>
    		</div>
    	);
    }

    function calculateWinner(squares) {
    	const lines = [
    		[0, 1, 2],
    		[3, 4, 5],
    		[6, 7, 8],
    		[0, 3, 6],
    		[1, 4, 7],
    		[2, 5, 8],
    		[0, 4, 8],
    		[2, 4, 6],
    	];
    	for (let i = 0; i < lines.length; i++) {
    		const [a, b, c] = lines[i];
    		if (
    			squares[a] &&
    			squares[a] === squares[b] &&
    			squares[a] === squares[c]
    		) {
    			return squares[a];
    		}
    	}
    	return null;
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/lmq6xq?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

### Отображение прошлых ходов {#showing-the-past-moves}

Поскольку вы записываете историю игры в крестики-нолики, теперь вы можете показать игроку список прошлых ходов.

Элементы React, такие как `<button>`, являются обычными объектами JavaScript; вы можете передавать их в своем приложении. Чтобы отобразить несколько элементов в React, вы можете использовать массив элементов React.

У вас уже есть массив движений `history` в state, теперь вам нужно преобразовать его в массив элементов React. В JavaScript для преобразования одного массива в другой можно использовать метод [массива `map`:](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

```js
[1, 2, 3].map((x) => x * 2); // [2, 4, 6]
```

Вы будете использовать `map` для преобразования вашей `history` ходов в элементы React, представляющие кнопки на экране, и отображения списка кнопок для "перехода" к прошлым ходам. Давайте сделаем `map` над `history` в компоненте `Game`:

```js hl_lines="13-15 17-31 43"
export default function Game() {
    const [xIsNext, setXIsNext] = useState(true);
    const [history, setHistory] = useState([
        Array(9).fill(null),
    ]);
    const currentSquares = history[history.length - 1];

    function handlePlay(nextSquares) {
        setHistory([...history, nextSquares]);
        setXIsNext(!xIsNext);
    }

    function jumpTo(nextMove) {
        // TODO
    }

    const moves = history.map((squares, move) => {
        let description;
        if (move > 0) {
            description = 'Go to move #' + move;
        } else {
            description = 'Go to game start';
        }
        return (
            <li>
                <button onClick={() => jumpTo(move)}>
                    {description}
                </button>
            </li>
        );
    });

    return (
        <div className="game">
            <div className="game-board">
                <Board
                    xIsNext={xIsNext}
                    squares={currentSquares}
                    onPlay={handlePlay}
                />
            </div>
            <div className="game-info">
                <ol>{moves}</ol>
            </div>
        </div>
    );
}
```

Ниже показано, как должен выглядеть ваш код. Обратите внимание, что в консоли инструментов разработчика вы должны увидеть ошибку, которая гласит:

!!!warning "Console"

    <pre style="white-space: normal">Warning: Each child in an array or iterator should have a unique "key" prop. Check the render method of 'Game'.</pre>

Вы исправите эту ошибку в следующем разделе.

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    function Board({ xIsNext, squares, onPlay }) {
    	function handleClick(i) {
    		if (calculateWinner(squares) || squares[i]) {
    			return;
    		}
    		const nextSquares = squares.slice();
    		if (xIsNext) {
    			nextSquares[i] = 'X';
    		} else {
    			nextSquares[i] = 'O';
    		}
    		onPlay(nextSquares);
    	}

    	const winner = calculateWinner(squares);
    	let status;
    	if (winner) {
    		status = 'Winner: ' + winner;
    	} else {
    		status = 'Next player: ' + (xIsNext ? 'X' : 'O');
    	}

    	return (
    		<>
    			<div className="status">{status}</div>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }

    export default function Game() {
    	const [xIsNext, setXIsNext] = useState(true);
    	const [history, setHistory] = useState([
    		Array(9).fill(null),
    	]);
    	const currentSquares = history[history.length - 1];

    	function handlePlay(nextSquares) {
    		setHistory([...history, nextSquares]);
    		setXIsNext(!xIsNext);
    	}

    	function jumpTo(nextMove) {
    		// TODO
    	}

    	const moves = history.map((squares, move) => {
    		let description;
    		if (move > 0) {
    			description = 'Go to move #' + move;
    		} else {
    			description = 'Go to game start';
    		}
    		return (
    			<li>
    				<button onClick={() => jumpTo(move)}>
    					{description}
    				</button>
    			</li>
    		);
    	});

    	return (
    		<div className="game">
    			<div className="game-board">
    				<Board
    					xIsNext={xIsNext}
    					squares={currentSquares}
    					onPlay={handlePlay}
    				/>
    			</div>
    			<div className="game-info">
    				<ol>{moves}</ol>
    			</div>
    		</div>
    	);
    }

    function calculateWinner(squares) {
    	const lines = [
    		[0, 1, 2],
    		[3, 4, 5],
    		[6, 7, 8],
    		[0, 3, 6],
    		[1, 4, 7],
    		[2, 5, 8],
    		[0, 4, 8],
    		[2, 4, 6],
    	];
    	for (let i = 0; i < lines.length; i++) {
    		const [a, b, c] = lines[i];
    		if (
    			squares[a] &&
    			squares[a] === squares[b] &&
    			squares[a] === squares[c]
    		) {
    			return squares[a];
    		}
    	}
    	return null;
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/wlg766?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Когда вы перебираете массив `history` внутри функции, которую вы передали `map`, аргумент `squares` перебирает каждый элемент `history`, а аргумент `move` перебирает каждый индекс массива: `0`, `1`, `2`, …. (В большинстве случаев вам понадобятся фактические элементы массива, но для вывода списка ходов вам понадобятся только индексы).

Для каждого хода в истории игры в крестики-нолики создается элемент списка `<li>`, который содержит кнопку `<button>`. У кнопки есть обработчик `onClick`, который вызывает функцию `jumpTo` (которую вы еще не реализовали).

На данный момент вы должны увидеть список ходов, произошедших в игре, и ошибку в консоли инструментов разработчика. Давайте обсудим, что означает ошибка "ключ".

### Выбор ключа {#picking-a-key}

При отображении списка React сохраняет некоторую информацию о каждом отображаемом элементе списка. Когда вы обновляете список, React должен определить, что изменилось. Вы могли добавить, удалить, перегруппировать или обновить элементы списка.

Представьте себе переход от

```html
<li>Alexa: 7 tasks left</li>
<li>Ben: 5 tasks left</li>
```

на

```html
<li>Ben: 9 tasks left</li>
<li>Claudia: 8 tasks left</li>
<li>Alexa: 5 tasks left</li>
```

В дополнение к обновленным подсчетам, человек, читающий это, вероятно, сказал бы, что вы поменяли местами порядок Alexa и Ben и вставили Claudia между Alexa и Ben. Однако React — это компьютерная программа, и она не может знать, что вы задумали, поэтому вам нужно указать свойство _key_ для каждого элемента списка, чтобы отличить каждый элемент списка от его братьев и сестер. Если бы ваши данные были из базы данных, то в качестве ключей можно было бы использовать идентификаторы базы данных Алексы, Бена и Клаудии.

```js hl_lines="1"
<li key={user.id}>
    {user.name}: {user.taskCount} tasks left
</li>
```

При повторном отображении списка React берет ключ каждого элемента списка и ищет совпадающий ключ в элементах предыдущего списка. Если в текущем списке есть ключ, которого не существовало ранее, React создает компонент. Если в текущем списке отсутствует ключ, который существовал в предыдущем списке, React уничтожает предыдущий компонент. Если два ключа совпадают, соответствующий компонент перемещается.

Ключи сообщают React об идентичности каждого компонента, что позволяет React сохранять состояние между повторными рендерингами. Если ключ компонента изменится, компонент будет уничтожен и создан заново с новым состоянием.

`key` — это специальное и зарезервированное свойство в React. Когда создается элемент, React извлекает свойство `key` и хранит ключ непосредственно в возвращаемом элементе. Несмотря на то, что `key` может выглядеть так, будто оно передается как `props`, React автоматически использует `key`, чтобы решить, какие компоненты обновлять. У компонента нет возможности узнать, какой `key` указал его родитель.

**Настоятельно рекомендуется назначать правильные ключи при построении динамических списков.** Если у вас нет соответствующего ключа, возможно, вам стоит перестроить данные таким образом, чтобы он у вас был.

Если ключ не указан, React сообщит об ошибке и по умолчанию будет использовать индекс массива в качестве ключа. Использование индекса массива в качестве ключа является проблематичным при попытке переупорядочить элементы списка или вставить/удалить элементы списка. Явная передача `key={i}` глушит ошибку, но имеет те же проблемы, что и индексы массивов, и не рекомендуется в большинстве случаев.

Ключи не обязательно должны быть глобально уникальными; они должны быть уникальными только между компонентами и их родственниками.

### Реализация путешествия во времени {#implementing-time-travel}

В истории игры крестики-нолики каждый прошлый ход имеет уникальный ID, связанный с ним: это порядковый номер хода. Ходы никогда не будут переупорядочены, удалены или вставлены в середину, поэтому безопасно использовать индекс хода в качестве ключа.

В функции `Game` вы можете добавить ключ как `<li key={move}>`, и если вы перезагрузите отрисованную игру, ошибка React "key" должна исчезнуть:

```js hl_lines="4"
const moves = history.map((squares, move) => {
    //...
    return (
        <li key={move}>
            <button onClick={() => jumpTo(move)}>
                {description}
            </button>
        </li>
    );
});
```

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    function Board({ xIsNext, squares, onPlay }) {
    	function handleClick(i) {
    		if (calculateWinner(squares) || squares[i]) {
    			return;
    		}
    		const nextSquares = squares.slice();
    		if (xIsNext) {
    			nextSquares[i] = 'X';
    		} else {
    			nextSquares[i] = 'O';
    		}
    		onPlay(nextSquares);
    	}

    	const winner = calculateWinner(squares);
    	let status;
    	if (winner) {
    		status = 'Winner: ' + winner;
    	} else {
    		status = 'Next player: ' + (xIsNext ? 'X' : 'O');
    	}

    	return (
    		<>
    			<div className="status">{status}</div>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }

    export default function Game() {
    	const [xIsNext, setXIsNext] = useState(true);
    	const [history, setHistory] = useState([
    		Array(9).fill(null),
    	]);
    	const currentSquares = history[history.length - 1];

    	function handlePlay(nextSquares) {
    		setHistory([...history, nextSquares]);
    		setXIsNext(!xIsNext);
    	}

    	function jumpTo(nextMove) {
    		// TODO
    	}

    	const moves = history.map((squares, move) => {
    		let description;
    		if (move > 0) {
    			description = 'Go to move #' + move;
    		} else {
    			description = 'Go to game start';
    		}
    		return (
    			<li key={move}>
    				<button onClick={() => jumpTo(move)}>
    					{description}
    				</button>
    			</li>
    		);
    	});

    	return (
    		<div className="game">
    			<div className="game-board">
    				<Board
    					xIsNext={xIsNext}
    					squares={currentSquares}
    					onPlay={handlePlay}
    				/>
    			</div>
    			<div className="game-info">
    				<ol>{moves}</ol>
    			</div>
    		</div>
    	);
    }

    function calculateWinner(squares) {
    	const lines = [
    		[0, 1, 2],
    		[3, 4, 5],
    		[6, 7, 8],
    		[0, 3, 6],
    		[1, 4, 7],
    		[2, 5, 8],
    		[0, 4, 8],
    		[2, 4, 6],
    	];
    	for (let i = 0; i < lines.length; i++) {
    		const [a, b, c] = lines[i];
    		if (
    			squares[a] &&
    			squares[a] === squares[b] &&
    			squares[a] === squares[c]
    		) {
    			return squares[a];
    		}
    	}
    	return null;
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/45tjv9?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Прежде чем реализовать `jumpTo`, необходимо, чтобы компонент `Game` отслеживал, какой шаг пользователь просматривает в данный момент. Для этого определите новую переменную состояния `currentMove`, по умолчанию равную `0`:

```js hl_lines="6"
export default function Game() {
    const [xIsNext, setXIsNext] = useState(true);
    const [history, setHistory] = useState([
        Array(9).fill(null),
    ]);
    const [currentMove, setCurrentMove] = useState(0);
    const currentSquares = history[history.length - 1];
    //...
}
```

Затем обновите функцию `jumpTo` внутри `Game`, чтобы обновить `currentMove`. Вы также установите `xIsNext` в `true`, если число, на которое вы меняете `currentMove`, четное.

```js hl_lines="4-5"
export default function Game() {
    // ...
    function jumpTo(nextMove) {
        setCurrentMove(nextMove);
        setXIsNext(nextMove % 2 === 0);
    }
    //...
}
```

Сейчас вы внесете два изменения в функцию `Game` `handlePlay`, которая вызывается, когда вы щелкаете по квадрату.

-   Если вы "вернетесь назад во времени" и затем сделаете новый ход с этой точки, вы захотите сохранить историю только до этого момента. Вместо добавления `nextSquares` после всех элементов (`...` spread syntax) в `history`, вы добавите его после всех элементов в `history.slice(0, currentMove + 1)`, чтобы сохранить только эту часть старой истории.
-   Каждый раз, когда выполняется перемещение, вам нужно обновлять `currentMove`, чтобы он указывал на последнюю запись истории.

```js hl_lines="2-7"
function handlePlay(nextSquares) {
    const nextHistory = [
        ...history.slice(0, currentMove + 1),
        nextSquares,
    ];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
    setXIsNext(!xIsNext);
}
```

Наконец, вы измените компонент `Game`, чтобы он отображал текущий выбранный ход, вместо того, чтобы всегда отображать последний ход:

```js hl_lines="7"
export default function Game() {
    const [xIsNext, setXIsNext] = useState(true);
    const [history, setHistory] = useState([
        Array(9).fill(null),
    ]);
    const [currentMove, setCurrentMove] = useState(0);
    const currentSquares = history[currentMove];

    // ...
}
```

Если вы нажмете на любой шаг в истории игры, доска "крестики-нолики" должна немедленно обновиться, чтобы показать, как выглядела доска после того, как этот шаг произошел.

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    function Board({ xIsNext, squares, onPlay }) {
    	function handleClick(i) {
    		if (calculateWinner(squares) || squares[i]) {
    			return;
    		}
    		const nextSquares = squares.slice();
    		if (xIsNext) {
    			nextSquares[i] = 'X';
    		} else {
    			nextSquares[i] = 'O';
    		}
    		onPlay(nextSquares);
    	}

    	const winner = calculateWinner(squares);
    	let status;
    	if (winner) {
    		status = 'Winner: ' + winner;
    	} else {
    		status = 'Next player: ' + (xIsNext ? 'X' : 'O');
    	}

    	return (
    		<>
    			<div className="status">{status}</div>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }

    export default function Game() {
    	const [xIsNext, setXIsNext] = useState(true);
    	const [history, setHistory] = useState([
    		Array(9).fill(null),
    	]);
    	const [currentMove, setCurrentMove] = useState(0);
    	const currentSquares = history[currentMove];

    	function handlePlay(nextSquares) {
    		const nextHistory = [
    			...history.slice(0, currentMove + 1),
    			nextSquares,
    		];
    		setHistory(nextHistory);
    		setCurrentMove(nextHistory.length - 1);
    		setXIsNext(!xIsNext);
    	}

    	function jumpTo(nextMove) {
    		setCurrentMove(nextMove);
    		setXIsNext(nextMove % 2 === 0);
    	}

    	const moves = history.map((squares, move) => {
    		let description;
    		if (move > 0) {
    			description = 'Go to move #' + move;
    		} else {
    			description = 'Go to game start';
    		}
    		return (
    			<li key={move}>
    				<button onClick={() => jumpTo(move)}>
    					{description}
    				</button>
    			</li>
    		);
    	});

    	return (
    		<div className="game">
    			<div className="game-board">
    				<Board
    					xIsNext={xIsNext}
    					squares={currentSquares}
    					onPlay={handlePlay}
    				/>
    			</div>
    			<div className="game-info">
    				<ol>{moves}</ol>
    			</div>
    		</div>
    	);
    }

    function calculateWinner(squares) {
    	const lines = [
    		[0, 1, 2],
    		[3, 4, 5],
    		[6, 7, 8],
    		[0, 3, 6],
    		[1, 4, 7],
    		[2, 5, 8],
    		[0, 4, 8],
    		[2, 4, 6],
    	];
    	for (let i = 0; i < lines.length; i++) {
    		const [a, b, c] = lines[i];
    		if (
    			squares[a] &&
    			squares[a] === squares[b] &&
    			squares[a] === squares[c]
    		) {
    			return squares[a];
    		}
    	}
    	return null;
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/lmwsyf?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

### Окончательная очистка {#final-cleanup}

Если вы внимательно посмотрите на код, вы можете заметить, что `xIsNext === true`, когда `currentMove` четное и `xIsNext === false`, когда `currentMove` нечетное. Другими словами, если вы знаете значение `currentMove`, то вы всегда можете определить, каким должно быть `xIsNext`.

Нет причин хранить оба этих значения в состоянии. На самом деле, всегда старайтесь избегать избыточного состояния. Упрощение того, что вы храните в состоянии, уменьшает количество ошибок и делает ваш код более понятным. Измените `Game` так, чтобы она не хранила `xIsNext` как отдельную переменную состояния, а определяла ее на основе `currentMove`:

```js hl_lines="6 15 19"
export default function Game() {
    const [history, setHistory] = useState([
        Array(9).fill(null),
    ]);
    const [currentMove, setCurrentMove] = useState(0);
    const xIsNext = currentMove % 2 === 0;
    const currentSquares = history[currentMove];

    function handlePlay(nextSquares) {
        const nextHistory = [
            ...history.slice(0, currentMove + 1),
            nextSquares,
        ];
        setHistory(nextHistory);
        setCurrentMove(nextHistory.length - 1);
    }

    function jumpTo(nextMove) {
        setCurrentMove(nextMove);
    }
    // ...
}
```

Вам больше не нужно объявлять состояние `xIsNext` или вызывать `setXIsNext`. Теперь у `xIsNext` нет шансов рассинхронизироваться с `currentMove`, даже если вы допустите ошибку при кодировании компонентов.

### Завершение работы {#wrapping-up}

Поздравляем! Вы создали игру "Крестики-нолики", которая:

-   Позволяет играть в крестики-нолики,
-   Указывает, когда игрок выиграл игру,
-   Сохраняет историю игры по мере ее прохождения,
-   Позволяет игрокам просматривать историю игры и видеть предыдущие версии игрового поля.

Отличная работа! Мы надеемся, что теперь у вас есть представление о том, как работает React.

Посмотрите на конечный результат здесь:

=== "App.js"

    ```js
    import { useState } from 'react';

    function Square({ value, onSquareClick }) {
    	return (
    		<button className="square" onClick={onSquareClick}>
    			{value}
    		</button>
    	);
    }

    function Board({ xIsNext, squares, onPlay }) {
    	function handleClick(i) {
    		if (calculateWinner(squares) || squares[i]) {
    			return;
    		}
    		const nextSquares = squares.slice();
    		if (xIsNext) {
    			nextSquares[i] = 'X';
    		} else {
    			nextSquares[i] = 'O';
    		}
    		onPlay(nextSquares);
    	}

    	const winner = calculateWinner(squares);
    	let status;
    	if (winner) {
    		status = 'Winner: ' + winner;
    	} else {
    		status = 'Next player: ' + (xIsNext ? 'X' : 'O');
    	}

    	return (
    		<>
    			<div className="status">{status}</div>
    			<div className="board-row">
    				<Square
    					value={squares[0]}
    					onSquareClick={() => handleClick(0)}
    				/>
    				<Square
    					value={squares[1]}
    					onSquareClick={() => handleClick(1)}
    				/>
    				<Square
    					value={squares[2]}
    					onSquareClick={() => handleClick(2)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[3]}
    					onSquareClick={() => handleClick(3)}
    				/>
    				<Square
    					value={squares[4]}
    					onSquareClick={() => handleClick(4)}
    				/>
    				<Square
    					value={squares[5]}
    					onSquareClick={() => handleClick(5)}
    				/>
    			</div>
    			<div className="board-row">
    				<Square
    					value={squares[6]}
    					onSquareClick={() => handleClick(6)}
    				/>
    				<Square
    					value={squares[7]}
    					onSquareClick={() => handleClick(7)}
    				/>
    				<Square
    					value={squares[8]}
    					onSquareClick={() => handleClick(8)}
    				/>
    			</div>
    		</>
    	);
    }

    export default function Game() {
    	const [history, setHistory] = useState([
    		Array(9).fill(null),
    	]);
    	const [currentMove, setCurrentMove] = useState(0);
    	const xIsNext = currentMove % 2 === 0;
    	const currentSquares = history[currentMove];

    	function handlePlay(nextSquares) {
    		const nextHistory = [
    			...history.slice(0, currentMove + 1),
    			nextSquares,
    		];
    		setHistory(nextHistory);
    		setCurrentMove(nextHistory.length - 1);
    	}

    	function jumpTo(nextMove) {
    		setCurrentMove(nextMove);
    	}

    	const moves = history.map((squares, move) => {
    		let description;
    		if (move > 0) {
    			description = 'Go to move #' + move;
    		} else {
    			description = 'Go to game start';
    		}
    		return (
    			<li key={move}>
    				<button onClick={() => jumpTo(move)}>
    					{description}
    				</button>
    			</li>
    		);
    	});

    	return (
    		<div className="game">
    			<div className="game-board">
    				<Board
    					xIsNext={xIsNext}
    					squares={currentSquares}
    					onPlay={handlePlay}
    				/>
    			</div>
    			<div className="game-info">
    				<ol>{moves}</ol>
    			</div>
    		</div>
    	);
    }

    function calculateWinner(squares) {
    	const lines = [
    		[0, 1, 2],
    		[3, 4, 5],
    		[6, 7, 8],
    		[0, 3, 6],
    		[1, 4, 7],
    		[2, 5, 8],
    		[0, 4, 8],
    		[2, 4, 6],
    	];
    	for (let i = 0; i < lines.length; i++) {
    		const [a, b, c] = lines[i];
    		if (
    			squares[a] &&
    			squares[a] === squares[b] &&
    			squares[a] === squares[c]
    		) {
    			return squares[a];
    		}
    	}
    	return null;
    }
    ```

=== "CodeSandbox"

    <iframe src="https://codesandbox.io/embed/q2z497?view=Editor+%2B+Preview" style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;" title="react.dev" allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking" sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"></iframe>

Если у вас есть свободное время или вы хотите попрактиковать свои новые навыки работы с React, вот несколько идей по улучшению игры "Крестики-нолики", перечисленных в порядке возрастания сложности:

1.  Только для текущего хода показывать "Вы находитесь на ходу \#..." вместо кнопки.
2.  Переписать `Board`, чтобы использовать два цикла для создания квадратов вместо их жесткого кодирования.
3.  Добавить кнопку переключения, позволяющую сортировать ходы по возрастанию или убыванию.
4.  Когда кто-то выигрывает, выделять три клетки, которые привели к выигрышу (а когда никто не выигрывает, выводить сообщение о том, что результат ничейный).
5.  В списке истории ходов отображать местоположение каждого хода в формате (row, col).

На протяжении всего этого урока вы затрагивали такие понятия React, как элементы, компоненты, пропсы и состояние. Теперь, когда вы увидели, как эти концепции работают при создании игры, ознакомьтесь с [Мыслим как React](thinking-in-react.md), чтобы увидеть, как те же концепции React работают при создании пользовательского интерфейса приложения.

<small>:material-information-outline: Источник &mdash; [https://react.dev/learn/tutorial-tic-tac-toe](https://react.dev/learn/tutorial-tic-tac-toe)</small>
