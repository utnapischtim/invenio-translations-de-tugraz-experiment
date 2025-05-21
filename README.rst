..
    Copyright (C) 2025 Graz University of Technology.

    invenio-translations-de is free software; you can redistribute it and/or
    modify it under the terms of the MIT License; see LICENSE file for more
    details.

====================
 invenio-translations-de
====================

.. image:: https://github.com/inveniosoftware/invenio-translations-de/workflows/CI/badge.svg
        :target: https://github.com/inveniosoftware/invenio-translations-de/actions?query=workflow%3ACI+branch%3Amaster

.. image:: https://img.shields.io/github/tag/inveniosoftware/invenio-translations-de.svg
        :target: https://github.com/inveniosoftware/invenio-translations-de/releases

.. image:: https://img.shields.io/pypi/dm/invenio-translations-de.svg
        :target: https://pypi.python.org/pypi/invenio-translations-de

.. image:: https://img.shields.io/github/license/inveniosoftware/invenio-translations-de.svg
        :target: https://github.com/inveniosoftware/invenio-translations-de/blob/master/LICENSE

German translation bundle for InvenioRDM


Development
===========

Install
-------

Choose a version of search and database, then run:

.. code-block:: console

    uv pip install -e .


Update javascript translations
------------------------------

.. code-block:: console

    # fetch javascript resources from transifex with configuration tx_config
    # this command transforms the fetched messages.po files to one translations.json file in the output-directory
    invenio i18n fetch-from-transifex --token $TRANSIFEX_TOKEN --languages de --output-directory path/to/tmp/translations/ --transifex-config path/to/invenio_translations_de/invenio_translations_de/ui/tx_config

    # this step is then used to distribute the combined translations to the packages inside of the virtual environment. it would change editable installed packages
    # inside of a Dockerfile this step has to be done after `invenio webpack create` but for `invenio webpack build`.
    # for invenio-cli this PR: https://github.com/inveniosoftware/invenio-cli/pull/388/files is necessary
    # without the PR it is still possible to use it, but it has to be done manually before a `invenio-cli asssets build`
    invenio i18n distribute-js-translations --input-directory translations/frontend/

Update python/jinja translations
--------------------------------

.. code-block:: console

    # create a pot file from your virtual environment, pybabel is part of https://github.com/python-babel/babel
    pybabel extract -o translations/messages.pot .venv/lib/python3.13/site-packages/invenio_*

    # fetch python/jinja resources defined in tx_config from transifex,
    # this will create directories defined in tx_config to store the resource messages.po file
    invenio i18n fetch-from-transifex --token $TRANSIFEX_TOKEN --languages de --output-directory path/to/tmp/translations/ --transifex-config path/to/invenio_translations_de/translations/tx_config

    # concats all messsages.po file into one
    find translations/transifex/ -name 'messages.po' | msgcat -o translations/de/LC_MESSAGES/messages.po -

    # use a tool like https://github.com/vslavik/poedit to updated po file with pot
    # translate not translated strings and save to invenio_translations_de/translations/de/LC_MESSAGES/messages.po
