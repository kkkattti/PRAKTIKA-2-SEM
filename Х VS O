#include <SFML/Graphics.hpp>
#include <array>
#include <iostream>
#include <cmath>
#include <random>

const int WINDOW_SIZE = 600; // Размер окна по ширине и высоте
const int CELL_SIZE = WINDOW_SIZE / 3;
const int INFO_HEIGHT = 150;  // Увеличил для кнопок внизу

enum class Player { None, X, O };
enum class Mode { None, PvP, PvBot };

class TicTacToe {
public:
    bool gameStarted = false;
    bool choosingPlayer = false;
    bool showEndButtons = false;
    Mode gameMode = Mode::None;

    TicTacToe() {
        board.fill(Player::None);
        currentPlayer = Player::X;
        gameOver = false;
        winner = Player::None;
        winLineIndex = -1;

        background.setSize(sf::Vector2f(WINDOW_SIZE, WINDOW_SIZE));
        background.setFillColor(sf::Color(230, 230, 250)); // светло-лавандовый

        for (int i = 1; i <= 2; ++i) {
            sf::RectangleShape line(sf::Vector2f(3, WINDOW_SIZE));
            line.setPosition(i * CELL_SIZE - 1, 0);
            line.setFillColor(sf::Color(50, 50, 50));
            lines.push_back(line);

            sf::RectangleShape hline(sf::Vector2f(WINDOW_SIZE, 3));
            hline.setPosition(0, i * CELL_SIZE - 1);
            hline.setFillColor(sf::Color(50, 50, 50));
            lines.push_back(hline);
        }

        if (!font.loadFromFile("C:/Users/user/MySFMLProject/x64/arial.ttf")) {
            std::cerr << "Ошибка загрузки шрифта arial.ttf\n";
        }

        text.setFont(font);
        text.setCharacterSize(36);
        text.setFillColor(sf::Color(100, 0, 0));
        text.setPosition(10, WINDOW_SIZE + 20);

        // Кнопки выбора игрока
        playerXButton.setSize(sf::Vector2f(200, 60));
        playerXButton.setFillColor(sf::Color(180, 180, 255));
        playerXButton.setPosition(50, WINDOW_SIZE + 20);

        playerOButton.setSize(sf::Vector2f(200, 60));
        playerOButton.setFillColor(sf::Color(255, 180, 180));
        playerOButton.setPosition(WINDOW_SIZE - 250, WINDOW_SIZE + 20);

        playerXText.setFont(font);
        playerXText.setString("X");
        playerXText.setCharacterSize(30);
        playerXText.setFillColor(sf::Color::Black);
        centerTextInRect(playerXText, playerXButton.getGlobalBounds());

        playerOText.setFont(font);
        playerOText.setString("O");
        playerOText.setCharacterSize(30);
        playerOText.setFillColor(sf::Color::Black);
        centerTextInRect(playerOText, playerOButton.getGlobalBounds());

        // Кнопки выбора режима
        modePvPButton.setSize(sf::Vector2f(250, 60));
        modePvPButton.setFillColor(sf::Color(180, 255, 180));
        modePvPButton.setPosition(50, 20);

        modePvBotButton.setSize(sf::Vector2f(250, 60));
        modePvBotButton.setFillColor(sf::Color(180, 180, 255));
        modePvBotButton.setPosition(WINDOW_SIZE - 300, 20);

        modePvPText.setFont(font);
        modePvPText.setString("PLAY WITH A FRIEND");
        modePvPText.setCharacterSize(24);
        modePvPText.setFillColor(sf::Color::Black);
        centerTextInRect(modePvPText, modePvPButton.getGlobalBounds());

        modePvBotText.setFont(font);
        modePvBotText.setString("PLAY WITH BOT");
        modePvBotText.setCharacterSize(24);
        modePvBotText.setFillColor(sf::Color::Black);
        centerTextInRect(modePvBotText, modePvBotButton.getGlobalBounds());

        // Кнопки конца игры
        continueButton.setSize(sf::Vector2f(250, 60));
        continueButton.setFillColor(sf::Color(180, 255, 180));
        continueButton.setPosition(50, WINDOW_SIZE + 60);

        continueText.setFont(font);
        continueText.setString("CONTINUE");
        continueText.setCharacterSize(24);
        continueText.setFillColor(sf::Color::Black);
        centerTextInRect(continueText, continueButton.getGlobalBounds());

        quitButton.setSize(sf::Vector2f(250, 60));
        quitButton.setFillColor(sf::Color(255, 180, 180));
        quitButton.setPosition(WINDOW_SIZE - 300, WINDOW_SIZE + 60);

        quitText.setFont(font);
        quitText.setString("QUIT");
        quitText.setCharacterSize(24);
        quitText.setFillColor(sf::Color::Black);
        centerTextInRect(quitText, quitButton.getGlobalBounds());

        // Загружаем изображение для меню
        if (!menuTexture.loadFromFile("C:/Users/user/MySFMLProject/x64/Х О.jfif")) {
            std::cerr << "Ошибка загрузки меню изображения\n";
        }
        menuSprite.setTexture(menuTexture);

        // Подгоняем размер картинки под ширину окна (с сохранением пропорций)
        float scaleX = static_cast<float>(WINDOW_SIZE) / menuTexture.getSize().x;
        float scaleY = static_cast<float>(WINDOW_SIZE) / menuTexture.getSize().y;
        menuSprite.setScale(scaleX, scaleY);

        rng.seed(std::random_device{}());
    }

    void draw(sf::RenderWindow& window, float elapsedTime) {
    window.draw(background);

    // Проверка наведения на кнопки
    sf::Vector2i mousePos = sf::Mouse::getPosition(window);

    if (modePvPButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
        modePvPButton.setFillColor(sf::Color(150, 255, 150)); // Более яркий цвет
    } else {
        modePvPButton.setFillColor(sf::Color(180, 255, 180)); // Исходный цвет
    }

    if (modePvBotButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
        modePvBotButton.setFillColor(sf::Color(150, 150, 255)); // Более яркий цвет
    } else {
        modePvBotButton.setFillColor(sf::Color(180, 180, 255)); // Исходный цвет
    }

    if (playerXButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
        playerXButton.setFillColor(sf::Color(150, 150, 255)); // Более яркий цвет
    } else {
        playerXButton.setFillColor(sf::Color(180, 180, 255)); // Исходный цвет
    }

    if (playerOButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
        playerOButton.setFillColor(sf::Color(255, 150, 150)); // Более яркий цвет
    } else {
        playerOButton.setFillColor(sf::Color(255, 180, 180)); // Исходный цвет
    }

    if (continueButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
        continueButton.setFillColor(sf::Color(150, 255, 150)); // Более яркий цвет
    } else {
        continueButton.setFillColor(sf::Color(180, 255, 180)); // Исходный цвет
    }

    if (quitButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
        quitButton.setFillColor(sf::Color(255, 150, 150)); // Более яркий цвет
    } else {
        quitButton.setFillColor(sf::Color(255, 180, 180)); // Исходный цвет
    }

    // Рисуем меню выбора режима
    if (gameMode == Mode::None) {
        window.draw(menuSprite);
        window.draw(modePvPButton);
        window.draw(modePvPText);
        window.draw(modePvBotButton);
        window.draw(modePvBotText);
        return;
    }

        // Рисуем линии
        for (auto& line : lines) {
            window.draw(line);
        }

        // Рисуем крестики и нолики
        for (int i = 0; i < 9; ++i) {
            int row = i / 3;
            int col = i % 3;
            sf::Vector2f pos(col * CELL_SIZE, row * CELL_SIZE);

            if (board[i] == Player::X) {
                // Анимация при победе для клеток выигрышной линии
                if (gameOver && winner == Player::X && isCellInWinLine(i)) {
                    drawAnimatedX(window, pos, elapsedTime);
                }
                else {
                    drawX(window, pos);
                }
            }
            else if (board[i] == Player::O) {
                if (gameOver && winner == Player::O && isCellInWinLine(i)) {
                    drawAnimatedO(window, pos, elapsedTime);
                }
                else {
                    drawO(window, pos);
                }
            }
        }

        if (gameOver && winner != Player::None && winLineIndex != -1) {
            drawWinLine(window, winLineIndex, elapsedTime);
        }

        if (choosingPlayer) {
            window.draw(playerXButton);
            window.draw(playerXText);
            window.draw(playerOButton);
            window.draw(playerOText);
        }
        else {
            if (gameOver) {
                showEndButtons = true; // Показываем кнопки окончания игры

                std::string result;
                if (winner == Player::None) result = "GOOD JOB!";
                else if (winner == Player::X) result = "VICTORY X!";
                else if (winner == Player::O) result = "VICTORY O!";
                text.setString(result);
                text.setPosition(10, WINDOW_SIZE + 20);
                window.draw(text);

                window.draw(continueButton);
                window.draw(continueText);
                window.draw(quitButton);
                window.draw(quitText);
            }
            else {
                showEndButtons = false;
                std::string turn = (currentPlayer == Player::X) ? "(X)" : "(O)";
                text.setString(turn);
                text.setPosition(10, WINDOW_SIZE + 20);
                window.draw(text);
            }
        }
    }

    void handleClick(sf::Vector2i mousePos) {
        if (gameMode == Mode::None) {
            if (modePvPButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
                gameMode = Mode::PvP;
                choosingPlayer = true;
                gameStarted = false;
                reset();
                return;
            }
            if (modePvBotButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
                gameMode = Mode::PvBot;
                choosingPlayer = true;
                gameStarted = false;
                reset();
                return;
            }
            return;
        }

        if (choosingPlayer) {
            if (playerXButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
                currentPlayer = Player::X;
                botPlayer = (currentPlayer == Player::X) ? Player::O : Player::X;
                choosingPlayer = false;
                gameStarted = true;
                reset();
                return;
            }
            else if (playerOButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
                currentPlayer = Player::O;
                botPlayer = (currentPlayer == Player::X) ? Player::O : Player::X;
                choosingPlayer = false;
                gameStarted = true;
                reset();
                return;
            }
            return;
        }

        if (showEndButtons) {
            if (continueButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
                reset();
                gameStarted = false;
                showEndButtons = false;
                choosingPlayer = true;
                reset();
                return;
            }
            else if (quitButton.getGlobalBounds().contains(static_cast<sf::Vector2f>(mousePos))) {
                // Завершаем программу - сигнализируем снаружи, тут просто exit
                exit(0);
            }
        }

        if (!gameStarted || gameOver) return;

        int col = mousePos.x / CELL_SIZE;
        int row = mousePos.y / CELL_SIZE;
        if (col >= 0 && col < 3 && row >= 0 && row < 3) {
            int idx = row * 3 + col;
            if (board[idx] == Player::None) {
                board[idx] = currentPlayer;
                checkGameOver();
                if (!gameOver) {
                    switchPlayer();

                    if (gameMode == Mode::PvBot && currentPlayer == botPlayer) {
                        botMove();
                    }
                }
            }
        }
    }

    void reset() {
        board.fill(Player::None);
        gameOver = false;
        winner = Player::None;
        winLineIndex = -1;
        text.setString("");
    }

private:
    std::array<Player, 9> board;
    Player currentPlayer;
    Player botPlayer;
    bool gameOver;
    Player winner;
    int winLineIndex = -1;

    sf::RectangleShape background;
    std::vector<sf::RectangleShape> lines;

    sf::Font font;
    sf::Text text;

    sf::RectangleShape playerXButton;
    sf::RectangleShape playerOButton;
    sf::Text playerXText;
    sf::Text playerOText;

    sf::RectangleShape modePvPButton;
    sf::RectangleShape modePvBotButton;
    sf::Text modePvPText;
    sf::Text modePvBotText;

    sf::RectangleShape continueButton;
    sf::RectangleShape quitButton;
    sf::Text continueText;
    sf::Text quitText;

    sf::Texture menuTexture;
    sf::Sprite menuSprite;

    std::mt19937 rng;

    void centerTextInRect(sf::Text& text, const sf::FloatRect& rect) {
        sf::FloatRect textRect = text.getLocalBounds();
        text.setOrigin(textRect.left + textRect.width / 2.f, textRect.top + textRect.height / 2.f);
        text.setPosition(rect.left + rect.width / 2.f, rect.top + rect.height / 2.f);
    }
    void drawX(sf::RenderWindow& window, sf::Vector2f pos) {
        float thickness = 8.f;
        float length = CELL_SIZE * 0.7f; // чуть короче клетки
        sf::Vector2f center = pos + sf::Vector2f(CELL_SIZE / 2.f, CELL_SIZE / 2.f);

        sf::RectangleShape line1(sf::Vector2f(length, thickness));
        line1.setFillColor(sf::Color(0, 0, 128)); // Темно-синий цвет
        line1.setOrigin(length / 2.f, thickness / 2.f);
        line1.setPosition(center);
        line1.setRotation(45.f);

        sf::RectangleShape line2(sf::Vector2f(length, thickness));
        line2.setFillColor(sf::Color(0, 0, 128)); // Темно-синий цвет
        line2.setOrigin(length / 2.f, thickness / 2.f);
        line2.setPosition(center);
        line2.setRotation(-45.f);

        window.draw(line1);
        window.draw(line2);
    }

    void drawO(sf::RenderWindow& window, sf::Vector2f pos) {
        sf::CircleShape circle(CELL_SIZE / 2 - 10);
        circle.setPosition(pos + sf::Vector2f(10, 10));
        circle.setFillColor(sf::Color::Transparent);
        circle.setOutlineThickness(10);
        circle.setOutlineColor(sf::Color(128, 0, 0)); // Темно-красный цвет
        window.draw(circle);
    }


    bool isCellInWinLine(int idx) {
        static const int winLines[8][3] = {
            {0,1,2}, {3,4,5}, {6,7,8},
            {0,3,6}, {1,4,7}, {2,5,8},
            {0,4,8}, {2,4,6}
        };
        if (winLineIndex < 0 || winLineIndex > 7) return false;
        for (int i = 0; i < 3; ++i)
            if (winLines[winLineIndex][i] == idx) return true;
        return false;
    }

    void drawWinLine(sf::RenderWindow& window, int idx, float elapsedTime) {
        const float thickness = 10.f;
        // Меняем прозрачность от 100 до 255 по синусоиде времени
        int alpha = static_cast<int>(178 + 77 * std::sin(elapsedTime * 10)); // 178 +- 77, частота мерцания 6

        sf::Color lineColor(255, 0, 0, alpha);

        static const int winLines[8][3] = {
            {0,1,2}, {3,4,5}, {6,7,8},
            {0,3,6}, {1,4,7}, {2,5,8},
            {0,4,8}, {2,4,6}
        };

        int c1 = winLines[idx][0];
        int c2 = winLines[idx][2];

        sf::Vector2f center1((c1 % 3) * CELL_SIZE + CELL_SIZE / 2.f,
            (c1 / 3) * CELL_SIZE + CELL_SIZE / 2.f);
        sf::Vector2f center2((c2 % 3) * CELL_SIZE + CELL_SIZE / 2.f,
            (c2 / 3) * CELL_SIZE + CELL_SIZE / 2.f);

        sf::Vector2f diff = center2 - center1;
        float length = std::sqrt(diff.x * diff.x + diff.y * diff.y) + 20.f;

        sf::RectangleShape line(sf::Vector2f(length, thickness));
        line.setFillColor(lineColor);
        line.setOrigin(0, thickness / 2.f);
        line.setPosition(center1);
        float angle = std::atan2(diff.y, diff.x) * 180.f / 3.14159265f;
        line.setRotation(angle);

        window.draw(line);
    }


    void drawAnimatedX(sf::RenderWindow& window, sf::Vector2f pos, float elapsedTime) {
        // Анимация увеличения и центрирования
        float t = std::fmod(elapsedTime * 2.f, 1.f); // скорость анимации
        float scale = 1.f + 0.5f * t; // увеличивается до 1.5x

        // Центр клетки
        sf::Vector2f center = pos + sf::Vector2f(CELL_SIZE / 2.f, CELL_SIZE / 2.f);

        sf::RectangleShape line1(sf::Vector2f((CELL_SIZE - 20) * scale, 10));
        line1.setFillColor(sf::Color(0, 0, 128));
        line1.setOrigin(line1.getSize().x / 2.f, line1.getSize().y / 2.f);
        line1.setPosition(center);
        line1.setRotation(45);

        sf::RectangleShape line2(sf::Vector2f((CELL_SIZE - 20) * scale, 10));
        line2.setFillColor(sf::Color(0, 0, 128));
        line2.setOrigin(line2.getSize().x / 2.f, line2.getSize().y / 2.f);
        line2.setPosition(center);
        line2.setRotation(-45);

        window.draw(line1);
        window.draw(line2);
    }

    void drawAnimatedO(sf::RenderWindow& window, sf::Vector2f pos, float elapsedTime) {
        float t = std::fmod(elapsedTime * 2.f, 1.f);
        float scale = 1.f + 0.5f * t;

        sf::Vector2f center = pos + sf::Vector2f(CELL_SIZE / 2.f, CELL_SIZE / 2.f);

        float radius = (CELL_SIZE / 2 - 10) * scale;
        sf::CircleShape circle(radius);
        circle.setOrigin(radius, radius);
        circle.setPosition(center);
        circle.setFillColor(sf::Color::Transparent);
        circle.setOutlineThickness(10);
        circle.setOutlineColor(sf::Color(128, 0, 0));
        window.draw(circle);
    }

    void checkGameOver() {
        static const int winLines[8][3] = {
            {0,1,2}, {3,4,5}, {6,7,8},
            {0,3,6}, {1,4,7}, {2,5,8},
            {0,4,8}, {2,4,6}
        };
        for (int i = 0; i < 8; ++i) {
            Player a = board[winLines[i][0]];
            Player b = board[winLines[i][1]];
            Player c = board[winLines[i][2]];
            if (a != Player::None && a == b && b == c) {
                gameOver = true;
                winner = a;
                winLineIndex = i;
                return;
            }
        }
        // Если нет победы, проверяем ничью
        bool full = true;
        for (auto p : board) {
            if (p == Player::None) {
                full = false;
                break;
            }
        }
        if (full) {
            gameOver = true;
            winner = Player::None;
            winLineIndex = -1;
        }
    }

    void switchPlayer() {
        currentPlayer = (currentPlayer == Player::X) ? Player::O : Player::X;
    }

    void botMove() {
        int bestScore = -1000;
        int bestMove = -1;

        for (int i = 0; i < 9; ++i) {
            if (board[i] == Player::None) {
                // Симулируем ход
                board[i] = currentPlayer;
                int score = minimax(0, false);
                board[i] = Player::None; // Откатываем ход

                if (score > bestScore) {
                    bestScore = score;
                    bestMove = i;
                }
            }
        }

        if (bestMove != -1) {
            board[bestMove] = currentPlayer;
            checkGameOver();
            if (!gameOver) switchPlayer();
        }
    }

    int minimax(int depth, bool isMaximizing) {
        if (checkWinner() == currentPlayer) return 10 - depth;  // Победа
        if (checkWinner() == (currentPlayer == Player::X ? Player::O : Player::X)) return depth - 10;  // Поражение
        if (isDraw()) return 0; // Ничья

        if (isMaximizing) {
            int bestScore = -1000;
            for (int i = 0; i < 9; ++i) {
                if (board[i] == Player::None) {
                    board[i] = currentPlayer;
                    bestScore = std::max(bestScore, minimax(depth + 1, false));
                    board[i] = Player::None;
                }
            }
            return bestScore;
        }
        else {
            int bestScore = 1000;
            Player opponent = (currentPlayer == Player::X) ? Player::O : Player::X;
            for (int i = 0; i < 9; ++i) {
                if (board[i] == Player::None) {
                    board[i] = opponent;
                    bestScore = std::min(bestScore, minimax(depth + 1, true));
                    board[i] = Player::None;
                }
            }
            return bestScore;
        }
    }

    Player checkWinner() {
        static const int winLines[8][3] = {
            {0,1,2}, {3,4,5}, {6,7,8},
            {0,3,6}, {1,4,7}, {2,5,8},
            {0,4,8}, {2,4,6}
        };

        for (auto& line : winLines) {
            if (board[line[0]] != Player::None && board[line[0]] == board[line[1]] && board[line[1]] == board[line[2]]) {
                return board[line[0]];
            }
        }
        return Player::None;
    }

    bool isDraw() {
        for (auto& cell : board) {
            if (cell == Player::None) return false;
        }
        return true;
    }
};

int main() {
    setlocale(LC_CTYPE, "RU");

    sf::RenderWindow window(sf::VideoMode(WINDOW_SIZE, WINDOW_SIZE + INFO_HEIGHT), "X VS O from Kate Afanasenko");

    // Загрузка иконки
    sf::Image icon;
    if (icon.loadFromFile("C:/Users/user/MySFMLProject/x64/LOGO")) {
        window.setIcon(icon.getSize().x, icon.getSize().y, icon.getPixelsPtr());
    }
    else {
        // Можно вывести предупреждение, если иконка не загрузилась
        // std::cerr << "Failed to load icon.png\n";
    }

    TicTacToe game;

    sf::Clock clock;

    while (window.isOpen()) {
        sf::Event event{};
        while (window.pollEvent(event)) {
            if (event.type == sf::Event::Closed)
                window.close();

            if (event.type == sf::Event::MouseButtonPressed) {
                if (event.mouseButton.button == sf::Mouse::Left) {
                    game.handleClick(sf::Mouse::getPosition(window));
                }
            }
        }

        float elapsedTime = clock.getElapsedTime().asSeconds();

        window.clear(sf::Color::White);
        game.draw(window, elapsedTime);
        window.display();
    }

    return 0;
} 
