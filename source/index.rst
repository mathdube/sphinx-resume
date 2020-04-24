
Mathieu Dubé
==============================================================

+---------------------+---------------------------------+
|**Email**            | math.dube@gmail.com             |
+---------------------+---------------------------------+
|**LinkedIn**         | www.linkedin.com/in/mathieudube |
+---------------------+---------------------------------+


**{{years_experience}} {{cv_data.text.years_experience}}**

**{{cv_data.profile.tag}}**

{{cv_data.text.latest_available}}  https://mathdube.github.io/sphinx-resume/html/{{index_name}}

.. toctree::
   :maxdepth: 2
   :caption: Contents:


{{ cv_data.profile.title }}
======================================================================

{{ cv_data.profile.text }}

{{ cv_data.experiences.title }}
=========================================================================

{% for c in cv_data.experiences.companies %}
{{c.name}}
#########################################################################
{{ c.from }} {{cv_data.text.until}} {{ c.until  }} | {{c.location}}

{% for t in c.titles %}


**{{t.title}}**

{% for a in t.activites %}
* {{a}}
{% endfor %} {# Activies #}
{% endfor %} {# Titles #}
{% endfor %} {# Companies #}

{{ cv_data.academics.title }}
=========================================================================
{% for p in cv_data.academics.programs %}
{{p.school}} - {{p.location}}
#########################################################################
**{{p.program}}**

Obtained {{ p.obtained  }}
{% endfor %}

{{ cv_data.skills.title }}
=========================================================================
{% for s in cv_data.skills.sections %}
**{{s.name}}** : {% for skill in s.skills  %} {{skill}}{{ "," if not loop.last }} {% endfor %}
{% endfor %}

.. note:: {{cv_data.text.created_with}}

  * Python {{python_version}}
  * Sphinx
  * Jinja2 templates.

  {{cv_data.text.project_source}} https://github.com/mathdube/sphinx-resume

  {{cv_data.text.html_version}} https://mathdube.github.io/sphinx-resume/html/{{index_name}}
