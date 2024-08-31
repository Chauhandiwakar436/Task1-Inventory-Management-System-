# Task1-Inventory-Management-System-
Inventory Management System
#include <iostream>
#include <fstream>
#include <vector>
#include <string>
#include <iomanip>

using namespace std;
class Product {
public:
    int id;
    string name;
    int quantity;
    double price;

  
    Product(int id, string name, int quantity, double price)
        : id(id), name(name), quantity(quantity), price(price) {}

    
    void display() const {
        cout << left << setw(10) << id << setw(20) << name << setw(10) << quantity << setw(10) << price << endl;
    }
};

class InventoryManagementSystem {
private:
    vector<Product> inventory;
    string filename;

 
    void loadFromFile() {
        ifstream file(filename);
        if (file.is_open()) {
            inventory.clear();
            int id, quantity;
            double price;
            string name;
            while (file >> id >> name >> quantity >> price) {
                inventory.push_back(Product(id, name, quantity, price));
            }
            file.close();
        }
    }

    // Save products to file
    void saveToFile() const {
        ofstream file(filename);
        if (file.is_open()) {
            for (const auto& product : inventory) {
                file << product.id << " " << product.name << " " << product.quantity << " " << product.price << endl;
            }
            file.close();
        }
    }

public:

    InventoryManagementSystem(string filename) : filename(filename) {
        loadFromFile();
    }

    void addProduct(int id, string name, int quantity, double price) {
        inventory.push_back(Product(id, name, quantity, price));
        saveToFile();
    
    void updateProduct(int id, int quantity, double price) {
        for (auto& product : inventory) {
            if (product.id == id) {
                product.quantity = quantity;
                product.price = price;
                saveToFile();
                return;
            }
        }
        cout << "Product not found!" << endl;
    }

 
    void deleteProduct(int id) {
        inventory.erase(remove_if(inventory.begin(), inventory.end(),
            [id](const Product& product) { return product.id == id; }), inventory.end());
        saveToFile();
    }

    void displayProducts() const {
        cout << left << setw(10) << "ID" << setw(20) << "Name" << setw(10) << "Quantity" << setw(10) << "Price" << endl;
        cout << "------------------------------------------------------------" << endl;
        for (const auto& product : inventory) {
            product.display();
        }
    }
};

int main() {
    InventoryManagementSystem ims("inventory.txt");

    int choice;
    do {
        cout << "\nInventory Management System" << endl;
        cout << "1. Add Product" << endl;
        cout << "2. Update Product" << endl;
        cout << "3. Delete Product" << endl;
        cout << "4. Display Products" << endl;
        cout << "5. Exit" << endl;
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1: {
            int id, quantity;
            double price;
            string name;
            cout << "Enter Product ID: ";
            cin >> id;
            cout << "Enter Product Name: ";
            cin >> name;
            cout << "Enter Product Quantity: ";
            cin >> quantity;
            cout << "Enter Product Price: ";
            cin >> price;
            ims.addProduct(id, name, quantity, price);
            break;
        }
        case 2: {
            int id, quantity;
            double price;
            cout << "Enter Product ID to Update: ";
            cin >> id;
            cout << "Enter New Quantity: ";
            cin >> quantity;
            cout << "Enter New Price: ";
            cin >> price;
            ims.updateProduct(id, quantity, price);
            break;
        }
        case 3: {
            int id;
            cout << "Enter Product ID to Delete: ";
            cin >> id;
            ims.deleteProduct(id);
            break;
        }
        case 4:
            ims.displayProducts();
            break;
        case 5:
            cout << "Exiting..." << endl;
            break;
        default:
            cout << "Invalid choice! Please try again." << endl;
        }
    } while (choice != 5);

    return 0;
}
