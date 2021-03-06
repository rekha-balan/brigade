# Releasing Brigade

Once the intended commit has been tested and we have confidence to cut a release,
we can follow these steps to release Brigade:

1. Issue a docs pull request with all `<current release>` strings updated to 
`<anticipated release>`, e.g. `0.19.0` becomes `0.20.0`.

    _(Note: amending [docs/index.yaml](../index.yaml) is not required.  This file still exists (as of writing)
    only to serve older chart versions for a period of time after the chart migration to
    [Azure/brigade-charts][brigade-charts] completed.)_

1. Once this pull request is merged, create and push the git tag from the intended commit:

    ```console
    $ git tag v0.20.0
    $ git push origin v0.20.0
    ```

    The release pipeline located in our [brigade.js](../../brigade.js) then takes over
    and does the heavy lifting of building component images, pushing to designated
    image registries, building the `brig` cli binaries and finally creating the
    [GitHub release](https://github.com/Azure/brigade/releases).

1. Lastly, issue a pull request in [Azure/brigade-charts][brigade-charts]
bumping the `version` and `appVersion` values in both the Brigade
[chart](https://github.com/Azure/brigade-charts/blob/master/charts/brigade/Chart.yaml) and
the Brigade Project [chart](https://github.com/Azure/brigade-charts/blob/master/charts/brigade-project/Chart.yaml)
and then re-building and re-indexing the charts.

    ```console
    $ # bump version and appVersion in charts/{brigade,brigade-project}/Chart.yaml
    $ make build index
    $ # commit changes, issue PR
    ```

[brigade-charts]: https://github.com/Azure/brigade-charts