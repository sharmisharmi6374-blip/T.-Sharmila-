from flask import Flask, render_template, request

app = Flask(__name__)


def calculate_bmi(weight, height):
    height_m = height / 100
    bmi = weight / (height_m * height_m)
    return round(bmi, 2)


def get_bmi_status(bmi):
    if bmi < 18.5:
        return "Underweight"
    elif bmi < 25:
        return "Normal"
    elif bmi < 30:
        return "Overweight"
    else:
        return "Obese"


def get_workout(goal, bmi):
    if goal == "Weight Loss":
        return [
            "30 minutes brisk walking",
            "15 minutes jogging",
            "Squats - 3 sets × 12 reps",
            "Push-ups - 3 sets × 10 reps",
            "Plank - 3 × 30 seconds"
        ]

    elif goal == "Muscle Gain":
        return [
            "Push-ups - 3 sets × 12 reps",
            "Squats - 4 sets × 12 reps",
            "Lunges - 3 sets × 10 reps",
            "Plank - 3 × 45 seconds",
            "Strength training - 30 minutes"
        ]

    else:
        return [
            "20 minutes walking",
            "Stretching - 10 minutes",
            "Light jogging - 15 minutes",
            "Yoga - 15 minutes",
            "Basic bodyweight exercises"
        ]


def get_diet(goal):
    if goal == "Weight Loss":
        return [
            "Oats and fruits for breakfast",
            "Vegetable salad",
            "Brown rice with vegetables",
            "Protein-rich foods",
            "Drink enough water"
        ]

    elif goal == "Muscle Gain":
        return [
            "Eggs and oats",
            "Chicken or paneer",
            "Rice with vegetables",
            "Milk or curd",
            "Nuts and fruits"
        ]

    else:
        return [
            "Balanced breakfast",
            "Fresh fruits",
            "Vegetables",
            "Whole grains",
            "Adequate water"
        ]


@app.route("/")
def home():
    return render_template("index.html")


@app.route("/predict", methods=["POST"])
def predict():

    name = request.form["name"]
    age = int(request.form["age"])
    weight = float(request.form["weight"])
    height = float(request.form["height"])
    goal = request.form["goal"]

    bmi = calculate_bmi(weight, height)
    status = get_bmi_status(bmi)

    workout = get_workout(goal, bmi)
    diet = get_diet(goal)

    return render_template(
        "result.html",
        name=name,
        age=age,
        weight=weight,
        height=height,
        goal=goal,
        bmi=bmi,
        status=status,
        workout=workout,
        diet=diet
    )


if __name__ == "__main__":
    app.run(debug=True)
