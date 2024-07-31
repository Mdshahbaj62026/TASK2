# TASK2
JAVA 

Overview
Stock: Represents a stock with a symbol and price.
Portfolio: Manages the user's balance and stock holdings.
StockMarket: Simulates the stock market, providing current stock prices.
TradingPlatform: Provides a user interface to interact with the platform.
1. Stock.java
This class represents a stock with its symbol and price.

java
Copy code
public class Stock {
    private String symbol;
    private double price;

    public Stock(String symbol, double price) {
        this.symbol = symbol;
        this.price = price;
    }

    public String getSymbol() {
        return symbol;
    }

    public double getPrice() {
        return price;
    }

    public void setPrice(double price) {
        this.price = price;
    }
}
2. Portfolio.java
This class manages the user's balance and stock holdings.

java
Copy code
import java.util.HashMap;

public class Portfolio {
    private double balance;
    private HashMap<String, Integer> holdings;

    public Portfolio(double initialBalance) {
        this.balance = initialBalance;
        this.holdings = new HashMap<>();
    }

    public double getBalance() {
        return balance;
    }

    public HashMap<String, Integer> getHoldings() {
        return holdings;
    }

    public void buyStock(String symbol, int quantity, double price) {
        if (balance >= price * quantity) {
            balance -= price * quantity;
            holdings.put(symbol, holdings.getOrDefault(symbol, 0) + quantity);
            System.out.println("Bought " + quantity + " shares of " + symbol + " at $" + price);
        } else {
            System.out.println("Insufficient balance to buy stock.");
        }
    }

    public void sellStock(String symbol, int quantity, double price) {
        if (holdings.containsKey(symbol) && holdings.get(symbol) >= quantity) {
            balance += price * quantity;
            holdings.put(symbol, holdings.get(symbol) - quantity);
            if (holdings.get(symbol) == 0) {
                holdings.remove(symbol);
            }
            System.out.println("Sold " + quantity + " shares of " + symbol + " at $" + price);
        } else {
            System.out.println("Insufficient shares to sell.");
        }
    }

    public void displayPortfolio() {
        System.out.println("Balance: $" + balance);
        System.out.println("Holdings:");
        for (String symbol : holdings.keySet()) {
            System.out.println(symbol + ": " + holdings.get(symbol) + " shares");
        }
    }
}
3. StockMarket.java
This class simulates the stock market by providing current stock prices.

java
Copy code
import java.util.HashMap;

public class StockMarket {
    private HashMap<String, Stock> stocks;

    public StockMarket() {
        this.stocks = new HashMap<>();
        initializeMarket();
    }

    private void initializeMarket() {
        stocks.put("AAPL", new Stock("AAPL", 150.00));
        stocks.put("GOOGL", new Stock("GOOGL", 2800.00));
        stocks.put("AMZN", new Stock("AMZN", 3400.00));
        stocks.put("MSFT", new Stock("MSFT", 290.00));
        stocks.put("TSLA", new Stock("TSLA", 700.00));
    }

    public Stock getStock(String symbol) {
        return stocks.get(symbol);
    }

    public void displayMarketData() {
        System.out.println("Market Data:");
        for (String symbol : stocks.keySet()) {
            Stock stock = stocks.get(symbol);
            System.out.println(symbol + ": $" + stock.getPrice());
        }
    }
}
4. TradingPlatform.java
This class provides the main user interface for interacting with the trading platform.

java

import java.util.Scanner;

public class TradingPlatform {
    private Portfolio portfolio;
    private StockMarket stockMarket;
    private Scanner scanner;

    public TradingPlatform(double initialBalance) {
        this.portfolio = new Portfolio(initialBalance);
        this.stockMarket = new StockMarket();
        this.scanner = new Scanner(System.in);
    }

    public void start() {
        int option;

        do {
            displayMenu();
            option = scanner.nextInt();
            scanner.nextLine(); // consume newline

            switch (option) {
                case 1:
                    stockMarket.displayMarketData();
                    break;
                case 2:
                    buyStock();
                    break;
                case 3:
                    sellStock();
                    break;
                case 4:
                    portfolio.displayPortfolio();
                    break;
                case 5:
                    System.out.println("Exiting...");
                    break;
                default:
                    System.out.println("Invalid option. Please try again.");
            }
        } while (option != 5);
    }

    private void displayMenu() {
        System.out.println("1. Display market data");
        System.out.println("2. Buy stock");
        System.out.println("3. Sell stock");
        System.out.println("4. Display portfolio");
        System.out.println("5. Exit");
        System.out.print("Choose an option: ");
    }

    private void buyStock() {
        System.out.print("Enter stock symbol: ");
        String symbol = scanner.nextLine();
        Stock stock = stockMarket.getStock(symbol);
        if (stock != null) {
            System.out.print("Enter quantity to buy: ");
            int quantity = scanner.nextInt();
            scanner.nextLine(); // consume newline
            portfolio.buyStock(symbol, quantity, stock.getPrice());
        } else {
            System.out.println("Invalid stock symbol.");
        }
    }

    private void sellStock() {
        System.out.print("Enter stock symbol: ");
        String symbol = scanner.nextLine();
        Stock stock = stockMarket.getStock(symbol);
        if (stock != null) {
            System.out.print("Enter quantity to sell: ");
            int quantity = scanner.nextInt();
            scanner.nextLine(); // consume newline
            portfolio.sellStock(symbol, quantity, stock.getPrice());
        } else {
            System.out.println("Invalid stock symbol.");
        }
    }

    public static void main(String[] args) {
        TradingPlatform platform = new TradingPlatform(10000.00);
        platform.start();
    }
}
Explanation
Stock Class:

Represents a stock with a symbol and a price.
Methods to get and set the price of the stock.
Portfolio Class:

Manages the user's balance and stock holdings using a HashMap.
Methods to buy and sell stocks, update the balance, and display the portfolio.
StockMarket Class:

Simulates the stock market by maintaining a list of stocks and their prices.
Methods to get stock information and display market data.
TradingPlatform Class:

Provides the user interface for the trading platform.
Menu-driven interface for displaying market data, buying and selling stocks, and displaying the portfolio.
Uses a Scanner to get user input.
How to Run
Save each of the above classes in separate files: Stock.java, Portfolio.java, StockMarket.java, and TradingPlatform.java.
Open a terminal or command prompt and navigate to the directory where the files are saved.
Compile all the classes using javac *.java.
Run the program using java TradingPlatform.
