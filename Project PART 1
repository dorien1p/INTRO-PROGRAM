#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>
#include <vector>
using namespace std;

struct FoodItem {
    string name;
    double price;
};

void displayMenu(const vector<FoodItem>& items) {
    cout << "\n--- Food Stand Menu ---\n";
    for (int i = 0; i < items.size(); ++i) {
        cout << (i + 1) << ". " << items[i].name << " - $" << fixed << setprecision(2) << items[i].price << endl;
    }
    cout << "6. Exit\n";
}

bool loadMenuFromFile(const string& filename, vector<FoodItem>& items) {
    ifstream inputFile(filename);
    if (!inputFile) {
        return false;
    }

    FoodItem item;
    while (inputFile >> item.name >> item.price) {
        items.push_back(item);
    }
    inputFile.close();
    return true;
}

bool saveOrderToFile(const string& filename, double total, double salesTax, double finalTotal) {
    ofstream outputFile(filename, ios::app);
    if (!outputFile) {
        cout << "Error saving the order to file." << endl;
        return false;
    }
    outputFile << "Order Summary:\n";
    outputFile << "Subtotal: $" << fixed << setprecision(2) << total << endl;
    outputFile << "Sales Tax: $" << fixed << setprecision(2) << salesTax << endl;
    outputFile << "Total: $" << fixed << setprecision(2) << finalTotal << endl;
    outputFile << "-------------------------\n";
    outputFile.close();
    return true;
}

double getTotal(double total, int choice, int quantity, const vector<FoodItem>& items) {
    if (choice >= 1 && choice <= items.size()) {
        total += items[choice - 1].price * quantity;
    }
    return total;
}

int main() {
    vector<FoodItem> items;
    double total = 0.0;
    bool exit = false;
    double taxRate;

    if (!loadMenuFromFile("menu.txt", items)) {
        cout << "Error: Unable to load menu from file. Using default menu.\n";
        items.push_back({"Burger", 5.00});
        items.push_back({"Fries", 2.50});
        items.push_back({"Soda", 1.50});
        items.push_back({"Pizza", 8.00});
        items.push_back({"Hot Dog", 3.00});
    }

    cout << "Enter the sales tax rate (as a percentage, e.g., 5 for 5%): ";
    cin >> taxRate;

    while (taxRate < 0) {
        cout << "Please enter a valid tax rate (>= 0): ";
        cin >> taxRate;
    }

    while (!exit) {
        displayMenu(items);
        cout << "Enter the number of the item you want to order (1-" << items.size() << ") or 6 to exit: ";
        int choice;
        cin >> choice;

        if (choice == 6) {
            exit = true;
            double salesTax = total * (taxRate / 100);
            double finalTotal = total + salesTax;

            cout << "\nThank you for visiting the Food Stand!\n";
            cout << "Subtotal: $" << fixed << setprecision(2) << total << endl;
            cout << "Sales Tax (" << taxRate << "%): $" << fixed << setprecision(2) << salesTax << endl;
            cout << "Total Amount (with tax): $" << fixed << setprecision(2) << finalTotal << endl;

            if (saveOrderToFile("order_summary.txt", total, salesTax, finalTotal)) {
                cout << "Order has been saved to 'order_summary.txt'.\n";
            } else {
                cout << "Failed to save the order.\n";
            }

            break;
        }

        if (choice < 1 || choice > items.size()) {
            cout << "Invalid choice! Please select a valid item.\n";
            continue;
        }

        cout << "Enter the quantity for " << items[choice - 1].name << ": ";
        int quantity;
        cin >> quantity;

        if (quantity < 1) {
            cout << "Please enter a valid quantity.\n";
            continue;
        }

        total = getTotal(total, choice, quantity, items);

        cout << "Current Subtotal: $" << fixed << setprecision(2) << total << endl;
    }

    return 0;
}
