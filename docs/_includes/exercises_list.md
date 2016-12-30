{% comment %}
----------------------------------------------------------------------------

  Muodostaa kokoelman tehtäväluettelo sisältäen linkit ao. tehtäviin. 
  Sisällytetään osan index-etusivulle.
                                                            {% endcomment %}


{% for sivu in site[page.collection] %}{% if sivu.layout == "exercise_page" %}[{{sivu.title}}]({{site.baseurl}}{{sivu.url}})   
{% endif %}{% endfor %}

