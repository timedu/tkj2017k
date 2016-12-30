

{% comment %}
----------------------------------------------------------------------------

  Muodostaa kokoelmaluettelo sisältäen linkit kurssin osien 
  johdanto-sivuille. Sisällytetään kurssin etusivulle.
                                                            {% endcomment %}

{% for collection in site.collections %} {% if collection.otsikko %}
1. [{{collection.otsikko}}]({{collection.label}}/){% endif %}{% endfor %}

