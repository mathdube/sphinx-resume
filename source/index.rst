.. Mathieu's Resume documentation master file, created by
   sphinx-quickstart on Thu Apr  9 11:27:55 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Mathieu's Resume
==============================================================

**{{years_experience}} years of experience** when last compiled on {{today}}

**Security Specialist  - Identity and Access management - Web and API security**

**Devops and project delivery**

.. note:: This resume was created with Python, Sphinx and Jinja2

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
{{ c.from }} until {{ c.until  }} | {{c.location}}


**{{c.title}}**

{% for a in c.activites %}
* {{a}}
{% endfor %}
{% endfor %}

{{ cv_data.skills.title }}
=========================================================================
{% for s in cv_data.skills.sections %}
**{{s.name}}** : {% for skill in s.skills  %} {{skill}}{{ "," if not loop.last }} {% endfor %}
{% endfor %}
