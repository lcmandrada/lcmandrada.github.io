# Click

Structure
```
project
    __init__.py
    cli
        __init__.py
        base.py
        utils.py
        check               # command group
            __init__.py
            commands.py     # subcommands
```

`base.py`
```py
import click

from check import commands
from utils import configure_logging

@click.group()
@click.option('--debug', is_flag=True)
def main(debug):
    if debug:
        configure_logging(level=logging.DEBUG)
    else:
        configure_logging()


main.add_command(check)
```

`check/commands.py`  
`$ check test ...`
```py
import click

@click.group()
@click.pass_context
def check(ctx):
    """check command"""
    pass

@check.command()
@click.pass_context
@click.option(...)
def test(ctx, ...):
    """test subcommand"""
    ...
```
