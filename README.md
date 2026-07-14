# Company-Data-Processing-system
"""
report_generator.py
--------------------
import csv
import json


class ReportGenerator:
    """Builds and saves the final payroll report."""

    def __init__(self, people: list):
        """`people` is a list of Employee / Manager instances."""
        self.people = people

    def generate_summary(self) -> list:
        """
        Build a list of report rows: one dict per person, including
        calculated salary and the "High Income" / "Normal" classification.
        """
        summary = []
        for person in self.people:
            calculated_salary = person.calculate_salary()
            classification = "High Income" if calculated_salary > 50000 else "Normal"
            summary.append({
                "id": person.id,
                "name": person.name,
                "role": getattr(person, "role", "N/A"),
                "type": person.__class__.__name__,
                "calculated_salary": calculated_salary,
                "classification": classification,
            })
        return summary

    def print_summary(self, summary: list = None):
        """Pretty-print the summary to the console."""
        summary = summary if summary is not None else self.generate_summary()
        print(f"{'ID':<6}{'Name':<15}{'Role':<12}{'Type':<10}{'Salary':>12}  Classification")
        print("-" * 75)
        for row in summary:
            print(f"{row['id']:<6}{row['name']:<15}{row['role']:<12}{row['type']:<10}"
                  f"{row['calculated_salary']:>12,.2f}  {row['classification']}")

    def save_csv(self, path: str, summary: list = None):
        """Save the report as a CSV file."""
        summary = summary if summary is not None else self.generate_summary()
        fieldnames = ["id", "name", "role", "type", "calculated_salary", "classification"]
        with open(path, "w", newline="", encoding="utf-8") as f:
            writer = csv.DictWriter(f, fieldnames=fieldnames)
            writer.writeheader()
            writer.writerows(summary)

    def save_json(self, path: str, summary: list = None):
        """Save the report as a JSON file."""
        summary = summary if summary is not None else self.generate_summary()
        with open(path, "w", encoding="utf-8") as f:
            json.dump(summary, f, indent=2)