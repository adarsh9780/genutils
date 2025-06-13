```python
from pydantic import BaseModel, Field
from typing import get_args, get_origin, Union

def simplify_type(annotation):
    """Simplify type annotations for LLM-friendly output."""
    origin = get_origin(annotation)
    args = get_args(annotation)

    if origin is Union:
        arg_types = [simplify_type(arg) for arg in args if arg is not type(None)]
        return f"Optional[{', '.join(arg_types)}]" if type(None) in args else ', '.join(arg_types)
    elif origin is list or origin is List:
        return f"List[{simplify_type(args[0])}]"
    elif origin is dict or origin is Dict:
        return f"Dict[{simplify_type(args[0])}, {simplify_type(args[1])}]"
    elif hasattr(annotation, '__name__'):
        return annotation.__name__
    else:
        return str(annotation)

def generate_llm_prompt_schema(model: BaseModel) -> str:
    lines = []
    for name, field in model.model_fields.items():
        field_type = simplify_type(field.annotation)
        description = field.description or ""
        optional = "Optional" if not field.is_required() else "Required"
        lines.append(f"- {name} ({field_type}, {optional}): {description}")
    return "\n".join(lines)

# Example usage
class User(BaseModel):
    id: int
    name: str
    email: str = Field(..., description="User email address")
    is_active: bool = True
    tags: list[str] = []
    metadata: dict[str, str] = {}

prompt_schema = generate_llm_prompt_schema(User)
print(prompt_schema)
```






Here’s a concise “pitch deck” for why each of these audiences should adopt your natural-language pandas chatbot, organized by persona:

## Executive Summary

Your tool combines the power of a full pandas/Matplotlib backend with a simple, English-language interface—eliminating boilerplate, speeding insights, and uniting technical and non-technical users on one platform.

---

## 1. Data Scientists (Alteryx-savvy, Python-capable)

* **Accelerate ad-hoc analysis**: Rather than dragging and dropping components in Alteryx or writing repetitive setup code in Python, you can simply ask, “Show me the top 10 customers by revenue,” and get working pandas code instantly ([Orbit Analytics][1]).
* **Hybrid flexibility**: You retain the power of native pandas (and soon Polars) for heavy lifting, while the NLQ interface handles data-prep plumbing—so you never “reinvent the wheel” on imports, `groupby`, or plotting ([Kyligence][2]).
* **Seamless hand-off to production**: Generated code snippets can be copy-pasted into notebooks or CI pipelines, slashing time from prototype to production-ready script ([Medium][3]).
* **Reduced cognitive load**: By embedding only the top 60 relevant columns and managing context automatically, your brain stays focused on modeling rather than schema-hunting ([Veezoo][4]).

---

## 2. Business Analysts (Excel/Pivot-power users)

* **No Python required**: If you can write a pivot table in Excel, you can ask “What was our monthly churn rate by region?” and instantly receive both the data and a chart—no coding ([TARGIT][5]).
* **Familiar outputs**: Results are rendered as tables and Matplotlib charts (via base64), akin to Excel charts—no unfamiliar dashboards or learning curve ([Orbit Analytics][1]).
* **Self-service BI on demand**: Break free of IT ticket backlogs; get answers in seconds without waiting for data-engineering support ([MachEye][6]).
* **Insight democratization**: Empower every department to explore data directly, boosting cross-functional visibility and collaboration ([DATAVERSITY][7]).

---

## 3. Leadership (Decision-makers & First-line Ad-hoc Askers)

* **Lightning-fast strategic insights**: Execute high-level queries (“Show me QoQ revenue growth vs. budget”) in real time, cutting report generation from days to minutes ([monday.com][8]).
* **One interface for all**: Whether your next question comes from Sales, Finance, or Operations, everyone uses the same English-language chat UI—no more juggling multiple dashboards ([Domo][9]).
* **Data-backed confidence**: Executive-grade charts and tables appear on-the-fly, so you can present to stakeholders with up-to-date numbers and visualizations ([Medium][10]).
* **Scalable governance**: Session-based isolation ensures each team’s context and history remain separate, maintaining data security and auditability at enterprise scale ([Domo][11]).

---

**Bottom Line:** This tool marries the depth of pandas with the simplicity of plain English—delivering rapid prototyping for data scientists, turnkey self-service for analysts, and instant executive insights for leadership, all on one platform.

[1]: https://www.orbitanalytics.com/blog/mastering-natural-language-query-transforming-data-analytics/?utm_source=chatgpt.com "Mastering Natural Language Query (NLQ) - Orbit Analytics"
[2]: https://kyligence.io/blog/natural-language-query-in-data-analytics/?utm_source=chatgpt.com "Natural Language Query in Analytics: Overview and Examples"
[3]: https://medium.com/%40black_hat7781/python-vs-alteryx-unveiling-the-best-tool-for-your-data-odyssey-a8ca928db4ad?utm_source=chatgpt.com "Python vs Alteryx: Unveiling the Best Tool for Your Data Odyssey"
[4]: https://veezoo.com/blog/natural-language-query-nlq-in-business-intelligence-history-and-comparison/?utm_source=chatgpt.com "Natural Language Query (NLQ) in BI: A brief history and comparison"
[5]: https://www.targit.com/en/blog/benefits-of-self-service-analytics?utm_source=chatgpt.com "Discover Five Benefits of Self-Service Analytics - TARGIT Blog"
[6]: https://www.macheye.com/blog/7-major-benefits-of-self-service-analytics/?utm_source=chatgpt.com "7 Major Benefits of Self Service Analytics | MachEye"
[7]: https://www.dataversity.net/self-service-analytics-pros-and-cons/?utm_source=chatgpt.com "Self-Service Analytics: Pros and Cons - DATAVERSITY"
[8]: https://monday.com/blog/project-management/how-executive-dashboards-help-leaders-make-informed-decisions-with-confidence/?utm_source=chatgpt.com "Executive Dashboards: Make Fast, Smart Business Decisions"
[9]: https://www.domo.com/learn/article/everything-you-need-to-know-about-executive-dashboards?utm_source=chatgpt.com "Everything you need to know about executive dashboards - Domo"
[10]: https://medium.com/%40tyler_48883/creating-executive-dashboards-that-drive-decision-making-bb92172bc2e5?utm_source=chatgpt.com "Creating Executive Dashboards That Drive Decision Making - Medium"
[11]: https://www.domo.com/glossary/what-is-self-service-analytics?utm_source=chatgpt.com "What is self-service analytics? - Domo"
