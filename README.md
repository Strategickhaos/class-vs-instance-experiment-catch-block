# class-vs-instance-experiment-catch-block
#include <iostream>
#include <string>
#include <vector>
#include <stdexcept>

class Person {
private:
    int age;
public:
    explicit Person(int initialAge) : age(initialAge) {}

    int getAge() const {
        return age;
    }

    std::string getStatus() const {
        if (age < 13) {
            return "You are young.";
        }
        else if (age < 18) {
            return "You are a teenager.";
        }
        else {
            return "You are old.";
        }
    }

    void ageOneYear() {
        if (age >= 120) {
            throw std::out_of_range("Cannot age beyond 120 years.");
        }
        age++;
    }
};

int main() {
    std::vector<Person> people;

    int numCases;
    do {
        std::cout << "Enter the number of test cases: ";
        std::cin >> numCases;
        if (std::cin.fail()) {
            std::cin.clear();
            std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
            std::cerr << "Invalid input. Please enter a positive integer.\n";
            numCases = -1;
        }
    } while (numCases < 0);

    for (int i = 0; i < numCases; i++) {
        int age;
        do {
            std::cout << "Enter the age of person " << i + 1 << ": ";
            std::cin >> age;
            if (std::cin.fail()) {
                std::cin.clear();
                std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
                std::cerr << "Invalid input. Please enter a positive integer.\n";
                age = -1;
            }
        } while (age < 0);

        Person person(age);
        std::cout << "Initial status: " << person.getStatus() << std::endl;

        try {
            // Simulate 3 years passing
            for (int j = 0; j < 3; j++) {
                person.ageOneYear();
            }
        }
        catch (const std::exception& ex) {
            std::cerr << "Exception caught: " << ex.what() << std::endl;
            break;
        }

        std::cout << "Status after 3 years: " << person.getStatus() << std::endl << std::endl;
        people.push_back(person);
    }

    std::cout << "Summary of people: " << std::endl;
    for (const auto& person : people) {
        std::cout << "Age: " << person.getAge() << ", Status: " << person.getStatus() << std::endl;
    }

    return 0;
}
