Get Started
===========

Overview
--------

.. automodule:: abstract_relations
    :members:

**Django Abstract Relations** allows dynamic creation of intermediary models inside many-to-many relationships of
abstract (or non-abstract) models. This is possible by using a :class:`AbstractManyToManyField <abstract_relations.models.AbstractManyToManyField>`
in place of original :class:`ManyToManyField <django.db.models.ManyToManyField>`.

.. code-block:: python
    :linenos:
    :emphasize-lines: 19

    from django.db import models

    from abstract_relations.models import AbstractManyToManyField


    class Category(models.Model):
        name = models.CharField(max_length=100)


    class AbstractItemCategoryThrough(models.Model):
        category = models.ForeignKey(Category)
        # the second model foreign key will be automatically added

        class Meta:
            abstract = True

    class Pizza(models.Model):
        name = models.CharField(max_length=100)
        categories = AbstractManyToManyField(Category, through=AbstractItemCategoryThrough)


Above code results in creation of following class in the same module where ``AbstractItemCategoryThrough``
was defined and usage of that class in place of original ``Pizza.categories.through``:

.. code-block:: python

    class PizzaCategory(models.Model):
        category = models.ForeignKey(Category)
        pizza = models.ForeignKey(Pizza)

        # The model with AbstractManyToManyField can be referenced-back with 'self.abstract_item'
        @property
        def abstract_item(self):
            return self.pizza



Installation
------------

1. Install ``django_abstract_relations``::

        pip install django_abstract_relations


2. Use :class:`AbstractManyToManyField <abstract_relations.models.AbstractManyToManyField>` instead of
:class:`ManyToManyField <django.db.models.ManyToManyField>` there, wherever you want dynamically created intermediary models!
