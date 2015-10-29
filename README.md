# Wrapper around selectize for Polymer

Component providing a Polymer wrapper around [selectize.js](https://github.com/brianreavis/selectize.js). It has a basic Polymer look and feel using a `paper-input-container` as its main input element. This could be greatly extendend and enhanced.

Forked from [selectize-decorator](https://github.com/display-interactive/selectize-decorator).

## Usage

Please see the [demo](https://github.com/czellweg/selectize-wrapper/blob/master/demo/index.html) page for how to use this compoentn. A simple use case could be as follows:

```html
    ...
    <selectize-wrapper id="ajax1"
      ajax-options-url="cities.json"
      ajax-result-transformer="{{cityResultTransformer}}">
    </selectize-wrapper>
    ...
    <script>
    ...
    // defines the function to be used *before* selectize-wrapper is initialized
    this.cityResultTransformer = function(data) {
      return data.map(function(city) {
        return {value: city, text: city};
      });
    };
    ...
    </script
```

More options are available, see the next example:

```html
    ...
    <selectize-wrapper id="ajax2"
      value-field="url"
      label-field="name"
      search-fields="{{searchFields}}"
      load-fn="{{loadFn}}"
      render="{{renderObjGithub}}"
      max-items="100"
      load-throttle="200">
    </selectize-wrapper>
    ...

    <script>
    ...
    this.searchFields = ['name'];

    this.loadFn = function(query, callback) {
      if (!query.length) return callback();
      $.ajax({
        url: 'https://api.github.com/legacy/repos/search/' + encodeURIComponent(query),
        type: 'GET',
        error: function() {
          callback();
        },
        success: function(res) {
          callback(res.repositories.slice(0, 10));
        }
      });
    };

    this.renderObjGithub = {
      option: function(item, escape) {
        return '<div>' +
        '<span class="title">' +
        '<span class="name"><i class="icon ' + (item.fork ? 'fork' : 'source') + '"></i>' + escape(item.name) + '</span>' +
        '<span class="by">' + escape(item.username) + '</span>' + '</span>' +
        '<span class="description">' + escape(item.description) + '</span>' +
        '<ul class="meta">' +
        (item.language ? '<li class="language">' + escape(item.language) + '</li>' : '') +
        '<li class="watchers"><span>' + escape(item.watchers) + '</span> watchers</li>' +
        '<li class="forks"><span>' + escape(item.forks) + '</span> forks</li>' +
        '</ul>' +
        '</div>';
      }
    };
    ...
    </script>
```
