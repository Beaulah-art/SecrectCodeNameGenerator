import java.util.Scanner;

public class SecretCodeNameGenerator {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        String fullNameInput;

        System.out.println("Welcome to the Secret Code Name Generator!");

        // Loop to allow multiple name entries until "exit" is typed
        while (true) {
            System.out.println("\nEnter your full name (e.g., \"Alice Johnson\") or type \"exit\" to quit:");
            fullNameInput = scanner.nextLine();

            // Check for 'exit' command (case-insensitive and trimmed)
            if (fullNameInput.trim().equalsIgnoreCase("exit")) {
                System.out.println("Exiting the generator. Goodbye!");
                break; // Exit the loop
            }

            // Validate input: if empty after trimming, prompt again
            if (fullNameInput.trim().isEmpty()) {
                System.out.println("Error: Please enter a valid name.");
                continue; // Skip to the next iteration of the loop
            }

            System.out.println("Choose a mode: ");
            System.out.println("1. Hero Mode");
            System.out.println("2. Villain Mode");
            System.out.print("Enter 1 or 2: ");
            String modeChoice = scanner.nextLine();
            String mode;

            // Determine the mode based on user input
            if (modeChoice.equals("1")) {
                mode = "hero";
            } else if (modeChoice.equals("2")) {
                mode = "villain";
            } else {
                System.out.println("Invalid mode choice. Defaulting to Hero Mode.");
                mode = "hero"; // Default to hero mode if input is invalid
            }

            // Generate and display the code name
            generateAndDisplayCodeName(fullNameInput, mode);
        }

        scanner.close(); // Close the scanner when done
    }

    /**
     * Generates and displays the secret code name based on the full name and chosen mode.
     *
     * @param fullName The user's full name.
     * @param mode     The chosen mode ("hero" or "villain").
     */
    private static void generateAndDisplayCodeName(String fullName, String mode) {
        // Use Trim to leading and trailing spaces and convert to lowercase
        fullName = fullName.trim().toLowerCase();

        // Split the full name into first and last names
        String[] nameParts = fullName.split(" ");
        String firstName;
        String lastName;

        if (nameParts.length == 0) {
            System.out.println("Error: Could not parse name.");
            return;
        } else if (nameParts.length == 1) {
            firstName = nameParts[0];
            lastName = nameParts[0]; // If only one name, use it for both for consistent processing
        } else {
            firstName = nameParts[0];
            lastName = nameParts[nameParts.length - 1]; // The last part is considered the last name
        }

        //Capitalize the first letter of each name
        String capitalizedFirstName = capitalize(firstName);
        String capitalizedLastName = capitalize(lastName);

        //Reverse the last name
        String reversedLastName = new StringBuilder(lastName).reverse().toString();

        // Step 5: Create a secret code name
        // First 2 letters of the first name + reversed last name
        // Ensure substring doesn't go out of bounds if first name is less than 2 chars
        String firstTwoLetters = firstName.length() >= 2 ? firstName.substring(0, 2) : firstName;
        StringBuilder secretCodeName = new StringBuilder(firstTwoLetters).append(reversedLastName);

        //Add a special ending
        boolean containsX = secretCodeName.toString().contains("x");
        boolean endsWithVowel = endsWithVowel(secretCodeName.toString());

        if (containsX || endsWithVowel) {
            secretCodeName.append("X-Agent");
        } else {
            // Insert "-007" in the middle
            int middleIndex = secretCodeName.length() / 2;
            secretCodeName.insert(middleIndex, "-007");
        }

        // Apply mode-specific styling (simulated with text formatting for console)
        String finalCodeName = secretCodeName.toString();
        if (mode.equals("hero")) {
            finalCodeName = " Hero: " + finalCodeName + " ";
        } else if (mode.equals("villain")) {
            finalCodeName = " Villain: " + finalCodeName + " ";
        }

       // Compare first and last names alphabetically
        int comparisonResult = capitalizedFirstName.compareTo(capitalizedLastName);
        String alphabeticalComparisonText;
        if (comparisonResult < 0) {
            alphabeticalComparisonText = String.format("%s comes before %s alphabetically.", capitalizedFirstName, capitalizedLastName);
        } else if (comparisonResult > 0) {
            alphabeticalComparisonText = String.format("%s comes before %s alphabetically.", capitalizedLastName, capitalizedFirstName);
        } else {
            alphabeticalComparisonText = String.format("%s and %s are alphabetically the same.", capitalizedFirstName, capitalizedLastName);
        }

        // Step 7: Format and display the final output
        System.out.printf("Your secret code name is: %s%n", finalCodeName);
        System.out.println(alphabeticalComparisonText);
    }

    /**
     * Capitalizes the first letter of a given string and converts the rest to lowercase.
     *
     * @param name The string to capitalize.
     * @return The capitalized string.
     */
    private static String capitalize(String name) {
        if (name == null || name.isEmpty()) {
            return "";
        }
        return name.substring(0, 1).toUpperCase() + name.substring(1).toLowerCase();
    }

    /**
     * Checks if a string ends with a vowel (a, e, i, o, u), case-insensitive.
     *
     * @param s The string to check.
     * @return true if the string ends with a vowel, false otherwise.
     */
    private static boolean endsWithVowel(String s) {
        if (s == null || s.isEmpty()) {
            return false;
        }
        char lastChar = Character.toLowerCase(s.charAt(s.length() - 1));
        return "aeiou".indexOf(lastChar) != -1;
    }
}
