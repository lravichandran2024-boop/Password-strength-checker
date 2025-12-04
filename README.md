# Password-strength-checker

import re

def check_password_strength(password):
    """
    Evaluates the strength of a password based on length and complexity.
    Returns a tuple: (strength_rating, score, feedback_message)
    """
    # 1. Initialize Score and Feedback List
    score = 0
    feedback = []

    # 2. Check Length (Most Important Factor)
    if len(password) >= 12:
        score += 2
    elif len(password) >= 8:
        score += 1
    else:
        feedback.append("Password is too short. Aim for at least 12 characters.")

    # 3. Check for Character Variety (using Regular Expressions)
    # Checks for: Lowercase, Uppercase, Digits, and Special Characters

    # Check for Lowercase letters
    if re.search(r"[a-z]", password):
        score += 1
    else:
        feedback.append("Add lowercase letters (a-z).")

    # Check for Uppercase letters
    if re.search(r"[A-Z]", password):
        score += 1
    else:
        feedback.append("Add uppercase letters (A-Z).")

    # Check for Digits
    if re.search(r"\d", password):
        score += 1
    else:
        feedback.append("Add numbers (0-9).")

    # Check for Special Characters
    # The regex below checks for any character that is NOT a letter, number, or whitespace.
    if re.search(r"[^a-zA-Z0-9\s]", password):
        score += 1
    else:
        feedback.append("Add special characters (e.g., !@#$%^&*).")

    # 4. Determine Strength Rating based on Final Score
    if score >= 5:
        strength = "Very Strong"
    elif score == 4:
        strength = "Strong"
    elif score == 3:
        strength = "Medium"
    elif score == 2:
        strength = "Weak"
    else:
        strength = "Very Weak"
        # If the score is 0 or 1, ensure there is length feedback.

    # 5. Format Output Message
    if not feedback:
        message = "Excellent! This is a secure password."
    else:
        message = "To improve strength, please consider:"
        message += "\n" + "\n".join(f"- {item}" for item in feedback)

    return strength, score, message

# --- Example Usage ---

# Test Case 1: Strong Password
password_strong = "P@sswOrd123!"
strength, score, message = check_password_strength(password_strong)
print(f"Password: {password_strong}")
print(f"Strength: {strength} (Score: {score}/5)")
print(f"Feedback:\n{message}\n")

print("-" * 30)

# Test Case 2: Weak Password
password_weak = "secret"
strength, score, message = check_password_strength(password_weak)
print(f"Password: {password_weak}")
print(f"Strength: {strength} (Score: {score}/5)")
print(f"Feedback:\n{message}\n")
